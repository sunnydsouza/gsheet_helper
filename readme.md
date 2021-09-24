# gsheets_helper

Helper functions wrapper over the google sheets python api

## Getting started

### Install
```
pip install gsheets_helper
```

### Google credentials
Obtain a Google oauth2 credential file.

Refer appendix section

### Usage

#### Basics
Understanding spreadsheet_id and sheet_id.

Open you google sheet in a browser and check the url
```buildoutcfg
https://docs.google.com/spreadsheets/d/XXXX/edit#gid=YYYY

XXXX -> spreadsheet_id
YYYY -> sheet_id
Be default, the first sheet in a google sheet will always have sheet_id=0
```
#### Getting Started - initialization
```
from gsheets_helper import GSheetsHelper

gsheets_health = GSheetsHelper(SCOPES, "path/to/credentials.json")  # refer appendix on how to get credential.json

# Example 
SCOPES = ['https://www.googleapis.com/auth/spreadsheets',
          'https://www.googleapis.com/auth/fitness.activity.read',
          'https://www.googleapis.com/auth/fitness.activity.write',
          'https://www.googleapis.com/auth/fitness.nutrition.read',
          'https://www.googleapis.com/auth/fitness.nutrition.write',
          'https://www.googleapis.com/auth/fitness.sleep.read',
          'https://www.googleapis.com/auth/fitness.sleep.write'
          ]
gsheets_health = GSheetsHelper(SCOPES, "path/to/credentials.json")
```
#### Read the sheet from the provided range name and return the values
```
get_sheet_data(self, spreadsheet_id, range_name)

#Example usage
RANGE_NAME = 'Sample!A2:D'
r = get_sheet_data(spreadsheet_id, range_name))
print(r)
# convert to panda dataframe, if required
pd1 = pd.DataFrame(r, columns=["A", "B", "C", "D"])
print(pd1)
```
#### Appending row to existing sheet
```

gsheets_health.append_row_to_sheet(spreadsheet_id, range_name, body)

# Example 
values = [
    [
        col1val, col2val, col3val          #bascially a single row
    ]
    # Additional rows ...
]
body = {
   'values': values
}
gsheets_health.append_row_to_sheet(spreadsheet_id, range_name, body)
```

#### Read the auth token generated from the session.
```
# Primary usage is to use the auth token for further querying google apis
# Could also be useful to generate the token and then use in a different tool like Postman
get_auth_token(self)

# Example usage
    response = requests.post(
        "https://www.googleapis.com/fitness/v1/users/me/dataset:aggregate",
        headers={
            "Authorization": "Bearer " + getAuthToken(),
            "content-type": "application/json",
        },
        data=json.dumps({
            "aggregateBy": [{
                "dataTypeName": "com.google.sleep.segment"
            }],
            "startTimeMillis": 1631817000000,
            "endTimeMillis": 1631903340000
        })
    )
```

####  Get rows matching a particular string in a column
```
get_matching_rows(self, spreadsheet_id, range_name, column_to_search, string_to_search, columns)

#Example usage
matching_rows = gsheets_health.get_matching_rows(spreadsheet_id, range_name, "Date", d_string, columns)

```

#### Deleting an existing row in the sheet
```
#Can be used in conjuction to getting matching rows to delete any record

#Example usage
gsheets_health.delete_row_matching_row(spreadsheet_id, sheet_id, int(matching_rows[0]) + 1)

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

