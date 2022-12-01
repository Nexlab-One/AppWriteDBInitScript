# Imports
import os
from dotenv import load_dotenv
from appwrite.client import Client
from appwrite.services.databases import Databases

# Load Environment Variables:
load_dotenv()
_APP_ENDPOINT_URL = os.getenv('_APP_ENDPOINT_URL')
_APP_ENDPOINT = _APP_ENDPOINT_URL+os.getenv('_APP_ENDPOINT')
_APP_PROJECT_ID = os.getenv('_APP_PROJECT_ID')
_APP_API_KEY = os.getenv('_APP_API_KEY')
_DEFAULT_EMAIL_DOMAIN = os.getenv('_DEFAULT_EMAIL_DOMAIN')

# Init Appwrite Objects
appWriteClient = Client()
(appWriteClient
  .set_endpoint(f'{_APP_ENDPOINT}') # Your API Endpoint
  .set_project(f'{_APP_PROJECT_ID}') # Your project ID
  .set_key(f'{_APP_API_KEY}') # Your secret API key
)

databases = Databases(appWriteClient)

databaseDict = {
    'DatabaseName' : {
        'Collections': {
            'CollectionName': {
                'attributes' : [
                    {
                        'type': 'string',
                        'key': 'Argon-Header',
                        'size': 256,
                        'required': True,
                        'default': ''
                    },
                    {
                        'type': 'double',
                        'key': 'Max-Storage',
                        'required': False,
                        'default': 0.5
                    },
                    {
                        'type': 'integer',
                        'key': 'Support-Tier',
                        'required': True,
                        'default': None
                    },
                ]
            }
        }
    }
}
# Functions
def createAttribute(databaseID, collectionID, data):
    match data["type"] :
            case 'string':
                return databases.create_string_attribute(databaseID, collectionID, key = data["key"], size = data["size"], required = data["required"], default = data["default"])
            case 'integer':
                return databases.create_integer_attribute(databaseID, collectionID, key = data["key"], required = data["required"], default = data["default"])
            case 'double':
                return databases.create_float_attribute(databaseID, collectionID, key = data["key"], required = data["required"], default = data["default"])

# Create Databases, Collections and their attributes
for database in databaseDict:
    # Create Database
    result = databases.create(database, database)
    print(database)
    for collection in databaseDict[database]["Collections"]:
        # Create Collection
        result = databases.create_collection(database, collection, collection)
        print(collection)
        for attributes in databaseDict[database]["Collections"][collection]:
            for attribute in databaseDict[database]["Collections"][collection][attributes]:
                # Create Attributes
                result = createAttribute(database, collection, attribute)
