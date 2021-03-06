---
title: 如何使用批量听录 - 语音服务
titleSuffix: Azure Cognitive Services
description: 如果要听录存储（如 Azure Blob）中的大量音频，则批量听录是理想的选择。 使用专用 REST API 可以通过共享访问签名 (SAS) URI 指向音频文件并异步接收听录。
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: wolfma
ms.custom: devx-track-csharp
ms.openlocfilehash: 3b9a491f7546fbaa8722498b164bfa56353dfcfc
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89050176"
---
# <a name="how-to-use-batch-transcription"></a>如何使用批量听录

批量听录是一组 REST API 操作，可用于听录存储中的大量音频。 你可以使用典型的 URI 或共享访问签名 (SAS) URI 来指向音频文件，并异步接收脚本结果。 使用 3.0 API，你可以转录一个或多个音频文件，或处理整个存储容器。

可以使用批量听录 REST API 调用以下方法：

|    批量听录操作                                             |    方法    |    REST API 调用                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    创建一个新的听录。                                              |    POST      |    speechtotext/v3.0/transcriptions            |
|    检索经过身份验证的订阅的听录列表。    |    GET       |    speechtotext/v3.0/transcriptions            |
|    获取脱机听录支持的区域设置列表。              |    GET       |    speechtotext/v3.0/transcriptions/locales    |
|    更新由 ID 标识的听录的可变详细信息。    |    PATCH     |    speechtotext/v3.0/transcriptions/{id}       |
|    删除指定的听录任务。                                 |    DELETE    |    speechtotext/v3.0/transcriptions/{id}       |
|    获取由给定 ID 标识的听录。                        |    GET       |    speechtotext/v3.0/transcriptions/{id}       |
|    获取由给定 ID 标识的听录的结果文件。    |    GET       |    speechtotext/v3.0/transcriptions/{id}/files |

你可以查看和测试详细的 API，它以 [Swagger 文档](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)的形式提供。

此 API 不需要自定义终结点，并且没有并发要求。

批量听录作业是按“尽力而为”的原则安排的。
您无法估计作业将更改为 "正在运行" 状态的时间，但应在正常系统负载下的几分钟内发生。 一旦进入 "正在运行" 状态，就比音频运行时播放速度快。

## <a name="prerequisites"></a>先决条件

与语音服务的其他所有功能一样，需要按照[入门指南](get-started.md)通过 [Azure 门户](https://portal.azure.com)创建订阅密钥。

>[!NOTE]
> 若要使用批量听录，需要具备语音服务的标准订阅 (S0)。 免费订阅密钥 (F0) 无效。 有关详细信息，请参阅[定价和限制](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)。

如果计划自定义模型，请按照[声音自定义](how-to-customize-acoustic-models.md)和[语言自定义](how-to-customize-language-model.md)中的步骤操作。 若要在批量听录中使用所创建的模型，需要提供其模型位置。 可以在检查模型的详细信息（`self` 属性）时检索模型位置。 批量听录服务不需要已部署的自定义终结点。

## <a name="batch-transcription-api"></a>批量听录 API

批处理脚本 API 支持以下格式：

| 格式 | 编解码器 | 每个样本的位数 | 采样率             |
|--------|-------|---------|---------------------------------|
| WAV    | PCM   | 16 位  | 8 kHz 或 16 kHz，单声道或立体声 |
| MP3    | PCM   | 16 位  | 8 kHz 或 16 kHz，单声道或立体声 |
| OGG    | OPUS  | 16 位  | 8 kHz 或 16 kHz，单声道或立体声 |

对于立体声音频流，在听录期间会拆分左右声道。 正在为每个通道创建 JSON 结果文件。
若要创建有序的最终脚本，请使用每个查询文本生成的时间戳。

### <a name="configuration"></a>配置

配置参数以 JSON 形式（一个或多个单独文件）提供：

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

配置参数以 JSON 形式（处理整个存储容器）提供：

```json
{
  "contentContainerUrl": "<SAS URL to the Azure blob container to transcribe>",
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of container using default model for en-US"
}
```

下面的 JSON 指定在批处理脚本中使用的自定义定型模型：

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "model": {
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}"
  },
  "displayName": "Transcription of file using default model for en-US"
}
```


### <a name="configuration-properties"></a>配置属性

使用以下可选属性来配置听录：

:::row:::
   :::column span="1":::
      **参数**
   :::column-end:::
   :::column span="2":::
      **说明**
:::row-end:::
:::row:::
   :::column span="1":::
      `profanityFilterMode`
   :::column-end:::
   :::column span="2":::
      可选，默认为 `Masked` 。 指定如何处理识别结果中的不雅内容。 接受的值为 `None`（禁用不雅内容筛选）、`Masked`（将不雅内容替换为星号）、`Removed`（从结果中删除所有不雅内容）或 `Tags`（添加“不雅内容”标记）。
:::row-end:::
:::row:::
   :::column span="1":::
      `punctuationMode`
   :::column-end:::
   :::column span="2":::
      可选，默认为 `DictatedAndAutomatic` 。 指定如何处理识别结果中的标点。 接受的值为 `None`（禁用标点）、`Dictated`（暗示显式（口述）标点）、`Automatic`（让解码器处理标点），或 `DictatedAndAutomatic`（使用听写标点和自动标点）。
:::row-end:::
:::row:::
   :::column span="1":::
      `wordLevelTimestampsEnabled`
   :::column-end:::
   :::column span="2":::
      可选， `false` 默认为。 指定是否应将字级时间戳添加到输出。
:::row-end:::
:::row:::
   :::column span="1":::
      `diarizationEnabled`
   :::column-end:::
   :::column span="2":::
      可选， `false` 默认为。 指定应该对输入（预期为包含两个语音的单声道）执行分割聚类分析。 注意：需要 `wordLevelTimestampsEnabled` 将设置为 `true` 。
:::row-end:::
:::row:::
   :::column span="1":::
      `channels`
   :::column-end:::
   :::column span="2":::
      可选， `0` `1` 默认情况下转录。 要处理的信道号的数组。 可在此指定处理音频文件中可用通道的子集（例如只听录 `0`）。
:::row-end:::
:::row:::
   :::column span="1":::
      `timeToLive`
   :::column-end:::
   :::column span="2":::
      可选，默认情况下不删除。 完成脚本后自动删除转录的持续时间。 在 `timeToLive` 批量处理转录中非常有用，可确保最终将其删除 (例如 `PT12H` 12 小时) 。
:::row-end:::
:::row:::
   :::column span="1":::
      `destinationContainerUrl`
   :::column-end:::
   :::column span="2":::
      可选 URL，其中包含 [服务即席 SAS](../../storage/common/storage-sas-overview.md) 到 Azure 中的可写容器。 结果存储在此容器中。 **不**支持具有存储访问策略的 SAS。 如果未指定，Microsoft 会将结果存储在由 Microsoft 管理的存储容器中。 当通过调用[删除听录](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription)来删除听录时，结果数据也会被删除。
:::row-end:::

### <a name="storage"></a>存储

批处理脚本可以从公共可见 internet URI 读取音频，还可以使用 SAS URI 和 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)读取音频或写入转录。

## <a name="batch-transcription-result"></a>批处理脚本结果

对于每个音频输入，都会创建一个脚本结果文件。
[Get 转录 files](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptionFiles)操作返回此脚本的结果文件列表。 若要查找特定输入文件的听录文件，请使用 `kind` == `Transcription` 和 `name` == `{originalInputName.suffix}.json` 来筛选所有返回的文件。

每个脚本结果文件都具有以下格式：

```json
{
  "source": "...",                      // sas url of a given contentUrl or the path relative to the root of a given container
  "timestamp": "2020-06-16T09:30:21Z",  // creation time of the transcription, ISO 8601 encoded timestamp, combined date and time
  "durationInTicks": 41200000,          // total audio duration in ticks (1 tick is 100 nanoseconds)
  "duration": "PT4.12S",                // total audio duration, ISO 8601 encoded duration
  "combinedRecognizedPhrases": [        // concatenated results for simple access in single string for each channel
    {
      "channel": 0,                     // channel number of the concatenated results
      "lexical": "hello world",
      "itn": "hello world",
      "maskedITN": "hello world",
      "display": "Hello world."
    }
  ],
  "recognizedPhrases": [                // results for each phrase and each channel individually
    {
      "recognitionStatus": "Success",   // recognition state, e.g. "Success", "Failure"
      "channel": 0,                     // channel number of the result
      "offset": "PT0.07S",              // offset in audio of this phrase, ISO 8601 encoded duration 
      "duration": "PT1.59S",            // audio duration of this phrase, ISO 8601 encoded duration
      "offsetInTicks": 700000.0,        // offset in audio of this phrase in ticks (1 tick is 100 nanoseconds)
      "durationInTicks": 15900000.0,    // audio duration of this phrase in ticks (1 tick is 100 nanoseconds)
      
      // possible transcriptions of the current phrase with confidences
      "nBest": [
        {
          "confidence": 0.898652852,    // confidence value for the recognition of the whole phrase
          "speaker": 1,                 // if `diarizationEnabled` is `true`, this is the identified speaker (1 or 2), otherwise this property is not present
          "lexical": "hello world",
          "itn": "hello world",
          "maskedITN": "hello world",
          "display": "Hello world.",
          
          // if wordLevelTimestampsEnabled is `true`, there will be a result for each word of the phrase, otherwise this property is not present
          "words": [
            {
              "word": "hello",
              "offset": "PT0.09S",
              "duration": "PT0.48S",
              "offsetInTicks": 900000.0,
              "durationInTicks": 4800000.0,
              "confidence": 0.987572
            },
            {
              "word": "world",
              "offset": "PT0.59S",
              "duration": "PT0.16S",
              "offsetInTicks": 5900000.0,
              "durationInTicks": 1600000.0,
              "confidence": 0.906032
            }
          ]
        }
      ]    
    }
  ]
}
```

结果包含以下字段：

:::row:::
   :::column span="1":::
      **字段**
   :::column-end:::
   :::column span="2":::
      **内容**
:::row-end:::
:::row:::
   :::column span="1":::
      `lexical`
   :::column-end:::
   :::column span="2":::
      识别的实际单词。
:::row-end:::
:::row:::
   :::column span="1":::
      `itn`
   :::column-end:::
   :::column span="2":::
      已识别文本的反向文本规范化形式。 已应用缩写（“doctor smith”缩写为“dr smith”）、电话号码和其他转换。
:::row-end:::
:::row:::
   :::column span="1":::
      `maskedITN`
   :::column-end:::
   :::column span="2":::
      应用了亵渎内容屏蔽的 ITN 形式。
:::row-end:::
:::row:::
   :::column span="1":::
      `display`
   :::column-end:::
   :::column span="2":::
      已识别文本的显示形式。 包括添加的标点和大小写。
:::row-end:::

## <a name="speaker-separation-diarization"></a>Diarization) 的扬声器分离 (

分割聚类是将讲述人语音分隔成音频片段的过程。 批处理管道支持 diarization，并且能够识别 mono 通道录制中的两个扬声器。 此功能不适用于立体声录音。

对于每个听录短语，启用了分割聚类的听录的输出都包含一个 `Speaker` 项。 如果未使用 diarization，则 `Speaker` JSON 输出中不存在该属性。 对于分割聚类，我们支持两段语音，因此讲述人标识为 `1` 或 `2`。

若要请求 diarization，请将 `diarizationEnabled` 属性设置为， `true` 如下所示。

 ```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "diarizationEnabled": true,
    "wordLevelTimestampsEnabled": true,
    "punctuationMode": "DictatedAndAutomatic",
    "profanityFilterMode": "Masked"
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

如上述请求中的参数所示，必须启用单词级时间戳。

## <a name="best-practices"></a>最佳实践

批处理脚本服务可处理大量提交的转录。 可以通过 [Get 转录](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptions)查询转录的状态。
检索结果后，请定期从服务中调用[删除听录](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription)。 或者设置 `timeToLive` 属性以确保最终删除结果。

## <a name="sample-code"></a>代码示例

`samples/batch` 子目录内的 [GitHub 示例存储库](https://aka.ms/csspeech/samples)中提供了完整示例。

使用你的订阅信息、服务区域、指向要转录的音频文件的 URI 和模型位置（如果你使用的是自定义模型）来更新示例代码。

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptiondefinition)]

示例代码设置客户端并提交听录请求。 然后，它会轮询状态信息并输出关于听录进度的详细信息。

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptionstatus)]

有关上述调用的完整详细信息，请参阅 [Swagger 文档](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)。 有关此处所示的完整示例，请转到 `samples/batch` 子目录中的 [GitHub](https://aka.ms/csspeech/samples)。

此示例使用异步设置来 post 音频和接收脚本状态。
`PostTranscriptions`方法发送音频文件详细信息， `GetTranscriptions` 方法接收状态。
`PostTranscriptions` 返回句柄，`GetTranscriptions` 使用此句柄创建一个句柄来获取听录状态。

此示例代码未指定自定义模型。 该服务使用基线模型来听录一个或多个文件。 若要指定模型，可将自定义模型的模型引用传递到相同的方法。

> [!NOTE]
> 对于基线听录，无需声明基线模型的 ID。

## <a name="next-steps"></a>后续步骤

- [语音到文本 v3 API 参考](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription)
