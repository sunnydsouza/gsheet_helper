# gsheets_helper

Helper functions wrapper over the google sheets python api

## Getting started

### Install
```
pip install gsheets_helper
```

### Google credentials
Obtain a Google oauth2 or service account credential file.

If using service account authentication, ensure the service account email is added to the Google Sheet.

### Usage

```
from gsheets_helper import GSheetsHelper

# refer appendix on how to get credential.json
# Example scopes
# SCOPES = ['https://www.googleapis.com/auth/spreadsheets',
#          'https://www.googleapis.com/auth/fitness.activity.read',
#          'https://www.googleapis.com/auth/fitness.activity.write',
#          'https://www.googleapis.com/auth/fitness.nutrition.read',
#          'https://www.googleapis.com/auth/fitness.nutrition.write',
#          'https://www.googleapis.com/auth/fitness.sleep.read',
#          'https://www.googleapis.com/auth/fitness.sleep.write'
#          ]
gsheets_health = GSheetsHelper(SCOPES, "path/to/credentials.json")

# appending row to existing sheet
# Example body to be sent
#    values = [
#        [
#            col1val, col2val, col3val
#        ]
#        # Additional rows ...
#    ]
#    body = {
#        'values': values
#    }
gsheets_health.append_row_to_sheet(spreadsheet_id, range_name, body)

#deleting an existing row in the sheet
gsheets_health.delete_row_matching_row(spreadsheet_id, sheet_id, int(matching_rows[0]) + 1)



print(df.head())
```

## Appendix

### Generating google credentials

#### oauth2

* Authentication flow occurs on code execution - choose the google account that has access to the document.

##### Creating credential in Google Cloud console
https://console.cloud.google.com/apis/credentials -> Create credentials -> Oauthclient ID

Select options:
* Application type = **Other**
* Name = whatever you want
Creates and downloads credential file with name format `client_secret_xxx-xxx.apps.googleusercontent.com.json`

