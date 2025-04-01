## Introduction

Azure Document Intelligence is built on top of Azure AI Vision and uses Optical Character Recognition (OCR) feature, to submit photographed or scanned documents and extract their words, text and other content in JSON format.

## Azure AI Vision vs. Azure Document Intelligence

If you want to extract simple words and text from a picture of a form or document, without contextual information, Azure AI Vision OCR is an appropriate service to consider. You might want to use this service if you already have your own analysis code, for example. However, Azure AI Document Intelligence includes a more sophisticated analysis of documents. For example, it can identify key/value pairs, tables, and context-specific fields. If you want to deploy a complete document analysis solution that enables users to both extract and understand text, consider Azure AI Document Intelligence.

## Why use Azure Document Intelligence?

Forms are used to communicate information in every industry, every day. Many people still manually extract data from forms to exchange information.

Consider some of the instances when a person needs to process form data and store it e.g in a database:

-   When filing claims
-   When enrolling new patients in an online management system
-   When entering data from receipts to an expense report
-   When reviewing an operations report for anomalies
-   When selecting data from a report to give to a stakeholder

Without AI services, people need to manually extract data and write it into other databases. With AI services, data can be extracted and processed automatically.

## How it works

OCR captures document structure by creating bounding boxes around detected objects in an image. The locations of the bounding boxes are recorded as coordinates in relation to the rest of the page. Azure Document Intelligence services return bounding box data and other information in a structured form with the relationships from the original file.

![[json-output-sample.png]]

## Create Cloud Resource

1. In the [Azure portal](https://portal.azure.com/#home), select **Create a resource**.
2. In the **Search services and marketplace** box, type **Document Intelligence** and then press **Enter**.
3. In the **Document intelligence** page, select **Create**.
4. In the **Create Document intelligence** page, under **Project Details**, select your **Subscription** and either select an existing **Resource group** or create a new one.
5. Under **Instance details**, select a **Region** near your users.
6. In the **Name** textbox, type a unique name for the resource.
7. Select a **Pricing tier** and then select **Review + create**.
8. If the validation tests pass, select **Create**. Azure deploys the new Azure AI Document Intelligence resource.
