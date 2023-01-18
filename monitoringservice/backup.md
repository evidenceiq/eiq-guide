# Backup

Request by <b>`Postman tool`</b>

`POST` (IP):3061/api/backup/add

## Database

*Request params:*

### Real-time
```json
{
    "name": "name of process (optional)",
    "type": "RealTime",
    "ComponentType": "Database",
    "Specs": {
        "Server": "db-host or ip",
        "Username": "db-username",
        "EncryptedPassword": "db-password encrypted",
        "DatabaseName": "database will backup",
        "BackupFolderPath": "backup folder path"
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
        "period": "daily" // daily | weekly
    },
    "ComponentType": "Database",
    "Specs": {
        "Server": "db-host or ip",
        "Username": "db-username",
        "EncryptedPassword": "db-password encrypted",
        "DatabaseName": "database will backup",
        "BackupFolderPath": "backup folder path"
    }
}
```