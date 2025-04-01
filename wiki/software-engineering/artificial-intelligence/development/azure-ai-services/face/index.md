Face detection, analysis, and recognition are all common computer vision challenges for AI systems. The ability to detect when a person is present, identify a person's facial location, or recognize an individual based on their facial features is a key way in which AI systems can exhibit human-like behavior and build empathy with users.

There are two Azure AI services that you can use to build solutions that detect faces in images:

1. Azure AI Vision Service ([[Face Detection]] Computer Vision)
    1. Detect human faces in images and returning a bounding box for its location
2. Face AI Service
    1. Face detection (with bounding box).
    2. Comprehensive facial feature analysis (including head pose, presence of spectacles, blur, facial landmarks, occlusion and others).
    3. Face comparison and verification.
    4. Facial recognition.

![[face-options.png]]

Usage of facial recognition, comparison, and verification will require getting approved through a [Limited Access policy](https://aka.ms/cog-services-limited-access). You can read more about the [addition of this policy](https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/) to our Responsible AI standard. Facial recognition will be touched on in the rest of this module, but will be unavailable without applying for Limited Access.

## Understand considerations for face analysis

While all applications of artificial intelligence require considerations for responsible and ethical use, system that rely on facial data can be particularly problematic.

When building a solution that uses facial data, considerations include (but are not limited to):

-   **Data privacy and security**. Facial data is personally identifiable, and should be considered sensitive and private. You should ensure that you have implemented adequate protection for facial data used for model training and inferencing.
-   **Transparency**. Ensure that users are informed about how their facial data will be used, and who will have access to it.
-   **Fairness and inclusiveness**. Ensure that you face-based system cannot be used in a manner that is prejudicial to individuals based on their appearance, or to unfairly target individuals.
