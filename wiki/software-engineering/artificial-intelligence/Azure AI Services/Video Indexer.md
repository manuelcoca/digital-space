The **Azure Video Indexer** service is a visual tool designed to help you extract information from videos. It provides functionality that you can use for:

-   *Facial recognition* - detecting the presence of individual people in the image. This requires [Limited Access](https://aka.ms/cog-services-limited-access)approval.
-   *Optical character recognition* - reading text in the video.
-   *Speech transcription* - creating a text transcript of spoken dialog in the video.
-   *Topics* - identification of key topics discussed in the video.
-   *Sentiment* - analysis of how positive or negative segments within the video are.
-   *Labels* - label tags that identify key objects or themes throughout the video.
-   *Content moderation* - detection of adult or violent themes in the video.
-   *Scene segmentation* - a breakdown of the video into its constituent scenes.

The Video Analyzer service provides a portal website that you can use to upload, view, and analyze videos interactively.

![[video-indexer-portal.png]] You can use a free, standalone version of the Video Analyzer service (with some limitations), or you can connect it to an **Azure Media Services** resource in your Azure subscription for full functionality.

# Azure Video Indexer API

For example, a call to the `https://api.videoindexer.ai.your_endpoint/Videos` REST interface returns details of videos in your account, similar to the following JSON example:

Azure Video Indexer provides a REST API that you can subscribe to in order to get a subscription key. You can then use your subscription key to consume the REST API and automate video indexing tasks, such as uploading and indexing videos, retrieving insights, and determining endpoints for Video Analyzer widgets.

```
{
  "results": [
    {
      "accountId":  "1234abcd-9876fghi-0156kihb-00123",
      "id":  "a12345bc6",
      "name":  "Responsible AI",
      "description":  "Microsoft Responsible AI video",
      "created":  "2021-01-05T15:33:58.918+00:00",
      "lastModified":  "2021-01-05T15:50:03.123+00:00",
      "lastIndexed":  "2021-01-05T15:34:08.007+00:00",
      "processingProgress":  "100%",
      "durationInSeconds":  114,
      "sourceLanguage":  "en-US",
    }
  ],
}
```

---

# Resources

-   Trainings resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/15-computer-vision.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/15-computer-vision.html)
