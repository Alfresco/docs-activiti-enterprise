---
Title: Files
---

# Files
Files can be uploaded and used within a project. They are stored in the modeling database whilst a project is created. Once an application is deployed, a persistent volume is created and mounted and the files are copied to this volume so they are accessible by all services.

There are two [associated files](../modeling/projects.md#files) that refer to a file definition. The first is the file itself stored as binary and the other contains the metadata for the file in JSON format.

The JSON definition of a file metadata is:

```json
{
    "createdAt": "2019-11-12T20:11:26.826Z",
    "name": "approval-policy-final.docx",
    "uri": "file:/policy.bin",
    "content": {
        "sizeInBytes": 86075,
        "mimeType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
    }
}
```

Once a file has been defined in the Modeling Application it can be created as a [process variable](../modeling/processes/README.md#process-variables) and used within a process definition. Choose the process variable type of `file` and select the relevant file from the dropdown. 

