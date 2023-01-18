# Synchronize

Request by <b>`Postman tool`</b>

`POST` (IP):3061/api/synchronize/add

## Files

*Request params:*

### Real-time
```json
{
    "name": "name of process (optional)",
    "type": "RealTime",
    "ComponentType": "Files",
    "Specs": {
        "SourceFilesFolderPath": "Source folder path",
        "DestinationFilesFolderPath": "Destination folder path",
        "ExcludeCondition": { // optional
            "FileNames": [],
            "FolderNames": [],
            "FileExtensions": [],
            "FilePrefixNames": [],
            "FolderPrefixNames": []
        },
        "NumberOfThreads": "-" // default = 2
    }
}
```

### Schedule

```json
{
    "name": "name of process (optional)",
    "type": "Schedule",
    "schedule":{
        "hour": 16, // UTC hour
        "minute": 59, // UTC minute
        "DayOfWeeks":["sunday"], // sunday, monday, tuesday, wednesday, thursday, friday, saturday
        "period": "daily" // daily|weekly
    },
    "ComponentType": "Files",
    "Specs": {
        "SourceFilesFolderPath": "Source folder path",
        "DestinationFilesFolderPath": "Destination folder path",
        "ExcludeCondition": { // optional
            "FileNames": [],
            "FolderNames": [],
            "FileExtensions": [],
            "FilePrefixNames": [],
            "FolderPrefixNames": []
        },
        "NumberOfThreads": "-" // default = 2
    }
}
```