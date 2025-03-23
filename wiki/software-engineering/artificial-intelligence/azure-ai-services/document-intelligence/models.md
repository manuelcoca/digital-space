# Prebuilt Models

Prebuilt Models can be found on the marketplace and be used to quickly get started: ![[02-search-marketplace.png]]

Models overview: - **General document analysis models** - Best for unusual and unique documents: - Read - General document - Layout - For specific forms: - Invoice - Receipt - W-2 US tax declaration - ID Document - Business card - Health insurance card

![[prebuilt-model-comparisson.png]]

## Read

Use this model to extract words and lines from both printed and hand-written documents. It also detects the language used in the document. ![[04-read-model.png]]

## General document

Use this model to extract key-value pairs and tables in your documents. This model is also the only model that can extract following entities:

-   `Person`. The name of a person.
-   `PersonType`. A job title or role.
-   `Location`. Buildings, geographical features, geopolitical entities.
-   `Organization`. Companies, government bodies, sports clubs, musical bands, and other groups.
-   `Event`. Social gatherings, historical events, anniversaries.
-   `Product`. Objects bought and sold.
-   `Skill`. A capability belonging to a person.
-   `Address`. Mailing address for a physical location.
-   `Phone number`. Dialing codes and numbers for mobile phones and landlines.
-   `Email`. Email addresses.
-   `URL`. Webpage addresses.
-   `IP Address`. Network addresses for computer hardware.
-   `DateTime`. Calendar dates and times of day.
-   `Quantity`. Numerical measurements with their units.

![[04-general-document-model.png]]

## Layout

Use this model to extract text, tables, and structure information from forms. It can also recognize selection marks such as check boxes and radio buttons. ![[04-layout-model.png]]

## Invoice

Use this model to extract key information from sales invoices in English and Spanish. Commonly used fields that can be extracted reliably:

-   Customer name and reference ID
-   Purchase order number
-   Invoice and due dates
-   Details about the vendor, such as name, tax ID, physical address.
-   Similar details about the customer.
-   Billing and shipping addresses.
-   Amounts such as total tax, invoice total, and amount due. ![[04-invoice-model.png]]

## Receipt

Use this model to extract data from printed and handwritten receipts. Following fields can be recognised reliably:

-   Merchant details such a name, phone number, and address.
-   Amounts such as receipt total, tax, and tip.
-   The date and time of the transaction.
-   The name of the item.
-   The quantity of the item purchased.
-   The unit price of the item.
-   The total price for that quantity. ![[04-receipt-model.png]]

## ID document

Use this model to extract data from United States driver's licenses and international passports. Following field can be extracted reliably:

-   First and last names.
-   Personal information such as sex, date of birth, and nationality.
-   The country and region where the document was issued.
-   Unique numbers such as the document number and machine readable zone.
-   Endorsements, restrictions, and vehicle classifications.

## Business card

Use this model to extract names and contact details from business cards. Following field can be extracted reliably:

-   First and last names.
-   Postal addresses.
-   Email and website addresses.
-   Various telephone numbers. ![[04-business-card-model.png]]

# Custom Models

If the prebuilt models don't suit your purposes, you can create a custom model and train it to analyze the specific type of document users will send. To train a custom model, you must supply at least five examples of the completed form but the more examples you supply, the greater the confidence levels.

To train a custom model:

1. Store sample forms in an Azure blob container, along with JSON files containing layout and label field information.
    - You can generate an **ocr.json** file for each sample form using the Azure Document Intelligence's **Analyze document** function. Additionally, you need a single **fields.json** file describing the fields you want to extract, and a **labels.json** file for each sample form mapping the fields to their location in that form.
2. Generate a shared access security (SAS) URL for the container.
3. Use the **Build model** REST API function (or equivalent SDK method).
4. Use the **Get model** REST API function (or equivalent SDK method) to get the trained **model ID**.

**OR**

5. Use the Azure Document Intelligence Studio to label and train. There are two types of custom models:
    - **Custom template models** - Use when layout across documents is very similar but content varies. Accurately extract labeled key-value pairs, selection marks, tables, regions, and signatures from documents. Training only takes a few minutes, and more than 100 languages for printed text and 9 languages for handwritten text are supported.
    - **Custom neural models** - Use for semi-structured and unstructured forms. Those models are deep learned models that combine layout and language features to accurately extract labeled fields from documents.

# File input requirements

Azure Document Intelligence works on input documents that meet these requirements:

-   Format must be JPG, PNG, BMP, PDF (text or scanned), or TIFF.
-   The file size must be less than 500 MB for paid (S0) tier and 4 MB for free (F0) tier.
-   Image dimensions must be between 50 x 50 pixels and 10000 x 10000 pixels.
-   The total size of the training data set must be 500 pages or less.

More input requirements can be found in the [documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/form-recognizer/overview) for specific models.

---

# Resources

-   https://microsoftlearning.github.io/mslearn-ai-document-intelligence/Instructions/Labs/02-custom-document-intelligence.html
-   Training Resource: https://learn.microsoft.com/en-us/training/modules/work-form-recognizer/1-introduction
