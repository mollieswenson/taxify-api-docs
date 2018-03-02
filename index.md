# About Taxify Documents

Taxify simplifies tax time for taxpayers and their tax professional. Taxpayers upload and manage documents in their taxpayer Taxify account, and have the option to authorize their tax professional to access their Taxify account, giving them the ability to manage the taxpayer's documents. Taxpayers and authorized tax professionals can upload, delete, annotate, and categorize documents that are stored in taxpayer's Taxify accounts.

### About the Taxify Documents API

Taxify uses a RESTful API with action-oriented HTTP verbs (GET, POST, PUT, DELETE) and built-in HTTP features for things like authentication and authorization. All responses include an HTTP status code, header, and response body in JSON or XML.

###### Errors

Error responses include the HTTP error code and a message describing the reason for the error. Error messages vary depending on the endpoint.

###### Base URL

`http://api.taxify.com`

# Documents

Here's a brief summary of what you can do with these endpoints.

Purpose                                                                 | Description
----------------------------------------------------------------------- | --------------------------------------------------------------------------
[Get a document](#get)                                   | Retrieves a single document for the specified user.
[Get document annotations](#get-document-annotations)           | Retrieves the annotations associated with the specified document.
[Create annotations for a document](#create-document-annotations) | Creates an annotation on the specified document.
[Change a document's category](#change-document-category)           | Changes the category and subcategory (optional) of the specified document.
[Delete a document](#delete-a-document)                                 | Deletes the specified document.

### Get a document <a name="get"/>

Retrieves a single document for the specified user.

### URL

`GET /api/v1/users/{userid}/documents/getsingledocument`

### Parameters

Name         | Type    | Required? | Description
------------ | ------- | --------- | -----------------------------------------------------
`userid`     | integer | required  | The associated taxpayer user ID.
`documentid` | integer | required  | The document ID.
`download`   | boolean | optional  | If `true`, download is initiated. Default is `false`.

### Request

```curl
cURL -X GET "https://api.taxify.com/api/v1/users/3659/documents/getsingledocument?id=28404
-H "Authorization: Bearer token_here"
```

### Response

This endpint returs JSON structured like this:

```json
{
 "id": 31946,
 "key": "31258af6-54e9-4845-97e7-0802b437c36c",
 "name": "My w-2 document",
 "extension": "pdf",
 "contentType": "application/pdf",
 "isUploaded": true,
 "createdDateTime": "2017-04-05T15:27:22.6323957+00:00",
 "editedDateTime": null,
 "documentId": 10
}
```

# Get document annotations

Retrieves the annotations associated with the specified document.

### URL

`GET /api/v1/users/{userid}/documents/{documentid}/annotations`

### Parameters

Name         | Type    | Required? | Description
------------ | ------- | --------- | --------------------------------
`userid`     | integer | required  | The associated taxpayer user ID.
`documentid` | integer | required  | The document ID.

### Request

```curl
cURL -X GET "https://api.taxify.com/api/v1/users/3659/documents/28404/annotations
-H "Authorization: Bearer token_here"
```

### Response

This endpint returs XML structured like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xfdf xmlns="http://ns.adobe.com/xfdf/" xml:space="preserve">
  <pdf-info xmlns="http://www.pdftron.com/pdfinfo" version="2"/>
  <fields/>
  <xfdf>
    <text subject="Comment" page="0" flags="print" name="f8262b02-f7c8-2844-8b20-1035521fb2c3" title="Tom Taxpayer" date="D:20170405084240-07'00'" color="#FFFF00" icon="Comment">
      <contents-richtext>
        <body>
         <p dir="ltr">
           <span dir="ltr" style="font-size:10.0pt;textalign:left;color:#000000;font-weight:normal;font-style:normal">This is an annotation</span>
         </p>
         </body>
      </contents-richtext>
      <contents>This is an annotation</contents>
  </text>
</xfdf>
```

# Create document annotations

Creates an annotation on the specified document.

### URL

`POST /api/v1/users/{userid}/documents/{documentid}/annotations`

### Parameters

Name         | Type    | Required? | Description
------------ | ------- | --------- | ------------------------------------------
`userid`     | integer | required  | The associated taxpayer user ID.
`documentid` | integer | required  | The document ID.
`data`       | string  | required  | Formatting and content for the annotation.

### Request

```curl
cURL -X POST "https://api.taxify.com/api/v1/users/3659/documents/28404/annotations
-H "Authorization: Bearer token_here"
-H "Content-Type: Application/json"
-H "Accept: Application/xml"
-d "data: font-size:10.0pt;textalign:left"
```

###### Request Body

The request body must include a string describing RTF formatting, which will be used to style the annotations. Valid styling includes:

- `font-size`
- `extalign`
- `color`
- `font-weight`
- `font-style`

### Responses

This endpoint returs JSON structured like this:

- Succesful: `{ "httpStatusCode": 200 }`
- Failed (document doesn't exist): `{ "httpStatusCode": 404 }`

# Change document category

Changes the category and subcategory (optional) of the specified document.

### URL

`PUT /api/v1/users/{userid}/documents/{documentid}/changecategory`

### Parameters

Name           | Type    | Required? | Description
-------------- | ------- | --------- | --------------------------------
`userid`       | integer | required  | The associated taxpayer user ID.
`documentid`   | integer | required  | The document ID.
`categoryid`   | integer | required  | The category ID.
`suggestionid` | integer | optional  | The subcategory ID.

### Request

```curl
cURL -X PUT "https://api.taxify.com/api/v1/users/3659/documents/28404/changecategory?categoryid=11
-H "Authorization: Bearer token_here"
-H "Accept: Application/json"
-d "categoryid: 11"
```

### Response

On success, this endpoint returs JSON structured like this:

```json
{
 "id": 0,
 "value": "Category has been changed",
 "error": null
}
```

# Delete a document

Deletes the specified document.

### URL

`DELETE /api/v1/users/{userid}/documents/{documentid}/delete`

### Parameters

Name       | Type    | Required? | Description
---------- | ------- | --------- | --------------------------------
userid     | integer | required  | The associated taxpayer user ID.
documentid | integer | required  | The document ID.

### Request

```curl
cURL -X DELETE "https://api.taxify.com/api/v1/users/3659/documents/28404/delete
-H "Authorization: Bearer token_here"
-H "Accept: Application/json"
```

### Response

On success, this endpoint returs JSON structured like this:

```json
{
 "value": "Document deleted",
 "error": null
}
```
