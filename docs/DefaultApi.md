# dap_annotation_service.DefaultApi

All URIs are relative to *https://localhost:4000/*

Method | HTTP request | Description
------------- | ------------- | -------------
[**es_get**](DefaultApi.md#es_get) | **GET** /es | Annotate data being hosted on an ElasticSearch index
[**progress_id_get**](DefaultApi.md#progress_id_get) | **GET** /progress/{id} | Get progress for a given annotation using its ID
[**s3_get**](DefaultApi.md#s3_get) | **GET** /s3 | Annotate data being hosted on an AWS S3 bucket
[**status_get**](DefaultApi.md#status_get) | **GET** /status | Get current status of Annotation server

# **es_get**
> InlineResponse2002 es_get(index, domain=domain, include_meta_data=include_meta_data, new_field=new_field, workers=workers)

Annotate data being hosted on an ElasticSearch index

This endpoint expects to be able to access the specified ES index and domain. Users should also specify the field contained within the ES doc to be tused as input for the annotation process.

### Example
```python
from __future__ import print_function
import time
import dap_annotation_service
from dap_annotation_service.rest import ApiException
from pprint import pprint
# Configure HTTP basic authorization: basicAuth
configuration = dap_annotation_service.Configuration()
configuration.username = 'YOUR_USERNAME'
configuration.password = 'YOUR_PASSWORD'

# create an instance of the API class
api_instance = dap_annotation_service.DefaultApi(dap_annotation_service.ApiClient(configuration))
index = 'index_example' # str | Index on which the text to be annotated is hosted
domain = 'search-datavis-arxlive-copy-n6ltva3lqh7x7xb6ucpaqfb5a4.eu-west-2.es.amazonaws.com' # str | ElasticSearch domain on which the index is hosted (optional) (default to search-datavis-arxlive-copy-n6ltva3lqh7x7xb6ucpaqfb5a4.eu-west-2.es.amazonaws.com)
include_meta_data = true # bool | Whether to include metadata about the annotation process along with annotation results (optional) (default to true)
new_field = 'dbpedia_entities' # str | Name of key corresponding to newly created annotation field (optional) (default to dbpedia_entities)
workers = 4.0 # float | Number of EC2 instances created to handle annotation process (optional) (default to 4.0)

try:
    # Annotate data being hosted on an ElasticSearch index
    api_response = api_instance.es_get(index, domain=domain, include_meta_data=include_meta_data, new_field=new_field, workers=workers)
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DefaultApi->es_get: %s\n" % e)
```

### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **index** | **str**| Index on which the text to be annotated is hosted | 
 **domain** | **str**| ElasticSearch domain on which the index is hosted | [optional] [default to search-datavis-arxlive-copy-n6ltva3lqh7x7xb6ucpaqfb5a4.eu-west-2.es.amazonaws.com]
 **include_meta_data** | **bool**| Whether to include metadata about the annotation process along with annotation results | [optional] [default to true]
 **new_field** | **str**| Name of key corresponding to newly created annotation field | [optional] [default to dbpedia_entities]
 **workers** | **float**| Number of EC2 instances created to handle annotation process | [optional] [default to 4.0]

### Return type

[**InlineResponse2002**](InlineResponse2002.md)

### Authorization

[basicAuth](../README.md#basicAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **progress_id_get**
> InlineResponse2001 progress_id_get(id)

Get progress for a given annotation using its ID

Using the ID provided by the response to an annotation request, you can use this endpoint to retrieve information about the progress of the annotation. When the annotation has finished, you should also recieve an email notifying you of the fact.

### Example
```python
from __future__ import print_function
import time
import dap_annotation_service
from dap_annotation_service.rest import ApiException
from pprint import pprint

# create an instance of the API class
api_instance = dap_annotation_service.DefaultApi()
id = 'id_example' # str | ID provided by the annotation request's response

try:
    # Get progress for a given annotation using its ID
    api_response = api_instance.progress_id_get(id)
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DefaultApi->progress_id_get: %s\n" % e)
```

### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **str**| ID provided by the annotation request&#x27;s response | 

### Return type

[**InlineResponse2001**](InlineResponse2001.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **s3_get**
> InlineResponse2002 s3_get(field, s3_input_uri, s3_output_uri, id_field=id_field, output_format=output_format, output_processor=output_processor, include_meta_data=include_meta_data, new_field=new_field, workers=workers)

Annotate data being hosted on an AWS S3 bucket

The endpoint expects an input S3 URI and output S3 URI. The dataset to be annotated must adhere to a required schema, which is documented below.  The input dataset must be in `json` format, with the root level object being an array. Each item in the array must be an object, with an optional id field, and the `field` key that you speficy in the request body. For example, the following is a dataset which conforms to the correct input schema:  <pre><code>[  { \"id\": 1, \"description\": \"This is some test data to be annotated.\" },  { \"id\": 2, \"description\": \"This is some more test date to be annotated.\" } ]</code></pre>  The API will perform a number of checks to ensure that the data is available and that the dataset and request is well formed. However, please ensure that the input S3 URI exists, and that the output S3 URI does *not* exist, as the service will not overwrite any existing data on S3.

### Example
```python
from __future__ import print_function
import time
import dap_annotation_service
from dap_annotation_service.rest import ApiException
from pprint import pprint
# Configure HTTP basic authorization: basicAuth
configuration = dap_annotation_service.Configuration()
configuration.username = 'YOUR_USERNAME'
configuration.password = 'YOUR_PASSWORD'

# create an instance of the API class
api_instance = dap_annotation_service.DefaultApi(dap_annotation_service.ApiClient(configuration))
field = 'field_example' # str | Field for the value being annotated.
s3_input_uri = 's3_input_uri_example' # str | URI for bucket containing input dataset
s3_output_uri = 's3_output_uri_example' # str | URI for output bucket. Please ensure this bucket does not already exist
id_field = 'null' # str | Field corresponding to ids for the documents being annotated. If null, these IDs will be generated using the ElasticSearch API. (optional) (default to null)
output_format = 'array' # str | Output type, whether the root data type should be an aray or an object (optional) (default to array)
output_processor = 'default' # str | Final type of output. Different types are described in detail in the docs. (optional) (default to default)
include_meta_data = true # bool | Whether to include metadata about the annotation process along with annotation results (optional) (default to true)
new_field = 'dbpedia_entities' # str | Name of key corresponding to newly created annotation field (optional) (default to dbpedia_entities)
workers = 4.0 # float | Number of EC2 instances created to handle annotation process (optional) (default to 4.0)

try:
    # Annotate data being hosted on an AWS S3 bucket
    api_response = api_instance.s3_get(field, s3_input_uri, s3_output_uri, id_field=id_field, output_format=output_format, output_processor=output_processor, include_meta_data=include_meta_data, new_field=new_field, workers=workers)
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DefaultApi->s3_get: %s\n" % e)
```

### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **field** | **str**| Field for the value being annotated. | 
 **s3_input_uri** | **str**| URI for bucket containing input dataset | 
 **s3_output_uri** | **str**| URI for output bucket. Please ensure this bucket does not already exist | 
 **id_field** | **str**| Field corresponding to ids for the documents being annotated. If null, these IDs will be generated using the ElasticSearch API. | [optional] [default to null]
 **output_format** | **str**| Output type, whether the root data type should be an aray or an object | [optional] [default to array]
 **output_processor** | **str**| Final type of output. Different types are described in detail in the docs. | [optional] [default to default]
 **include_meta_data** | **bool**| Whether to include metadata about the annotation process along with annotation results | [optional] [default to true]
 **new_field** | **str**| Name of key corresponding to newly created annotation field | [optional] [default to dbpedia_entities]
 **workers** | **float**| Number of EC2 instances created to handle annotation process | [optional] [default to 4.0]

### Return type

[**InlineResponse2002**](InlineResponse2002.md)

### Authorization

[basicAuth](../README.md#basicAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **status_get**
> InlineResponse200 status_get()

Get current status of Annotation server

Returns the status of the current Annotation server. Includes information on provisioned EC2 instances for ongoing annotations. Also includes information about past annotations, and the progess of ongoing annotations.

### Example
```python
from __future__ import print_function
import time
import dap_annotation_service
from dap_annotation_service.rest import ApiException
from pprint import pprint

# create an instance of the API class
api_instance = dap_annotation_service.DefaultApi()

try:
    # Get current status of Annotation server
    api_response = api_instance.status_get()
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DefaultApi->status_get: %s\n" % e)
```

### Parameters
This endpoint does not need any parameter.

### Return type

[**InlineResponse200**](InlineResponse200.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

