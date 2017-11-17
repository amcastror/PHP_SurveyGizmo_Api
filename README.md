# Official PHP library for SurveyGizmo API

[![Latest Stable Version](https://poser.pugx.org/surveygizmo/surveygizmo-api/v/stable)](https://packagist.org/packages/surveygizmo/surveygizmo-api)
[![Total Downloads](https://poser.pugx.org/surveygizmo/surveygizmo-api/downloads)](https://packagist.org/packages/surveygizmo/surveygizmo-api)
[![Latest Unstable Version](https://poser.pugx.org/surveygizmo/surveygizmo-api/v/unstable)](https://packagist.org/packages/surveygizmo/surveygizmo-api)
[![License](https://poser.pugx.org/surveygizmo/surveygizmo-api/license)](https://packagist.org/packages/surveygizmo/surveygizmo-api)
[![composer.lock](https://poser.pugx.org/surveygizmo/surveygizmo-api/composerlock)](https://packagist.org/packages/surveygizmo/surveygizmo-api)

## Summary
The library is intended to make integrating with SurveyGizmo easier and quicker than using the API directly.  The following objects are supported via this library and are all namespaced under SurveyGizmo (eg; \SurveyGizmo\Resources\Survey).

##### Supported Objects
- Survey
  - Responses
  - Questions
  - Pages
  - Statistics
  - Reports
  - Campaigns
    - EmailMessages
- Account
  - Users
  - Teams
- Contacts
- ContactLists
	- Contacts


####All objects use the following standard functions:

```
<OBJECT>::fetch(<FILTERS>,<OPTIONS>);
```
> Returns an array of objects based on filter and paging options.

```
<OBJECT>::get($id);
```
> Returns a single object based on id

```
<OBJECT>->save();
```
> Saves a newly created or updated instance of an object

```
<OBJECT>->delete();
```
> Deletes an instance of an object


## Code Examples

#### Auto Loading & Authenticating
If you are not using composer, you can use our AutoLoader directly as follows:
```php
require_once "<LIBRARY_PATH>/SurveyGizmoAutoLoader.php";

try {
	\SurveyGizmo\SurveyGizmoAPI::auth("<YOUR API_KEY>", "<YOUR API_SECRET>");
} catch (Exception $e) {
	die("Error Authenticating");
}
```

#### Rate limiting
For info on API request limiting see https://apihelp.surveygizmo.com/help/api-request-limits.
```php
//set max retries of requests to 10, when request is rate limited it will be retried after 5 seconds.
\SurveyGizmo\ApiRequest::setRepeatRateLimitedRequest(10);
```

### Surveys

###### Fetching Surveys
See filter and paging below.
```php
$surveys = \SurveyGizmo\Resources\Survey::fetch(<FILTER>,<OPTIONS>);
```

###### Getting a Single Survey
```php
$survey_id = <SURVEY_ID>;
$survey = \SurveyGizmo\Resources\Survey::get(survey_id);
```

###### Updating a Survey
```php
$survey->title = "TEST UPDATE FROM API LIBRARY";
$survey->save();
```

###### Creating a Survey
```php
$survey = new \SurveyGizmo\Resources\Survey();
$survey->title = "NEW SURVEY";
$results = $survey->save();
```

###### Deleting a Survey
```php
$survey = $survey->delete();
```
### Survey Helper Functions
The Survey object provides a few help functions to easily access related collections and objects.

```php
	//get questions
	$survey->getQuestions(<FILTER>,<PAGE>);
	$survey->getQuestion($question_id);
	//get responses
	$survey->getResponses(<FILTER>,<PAGE>);
	$survey->getResponse($id);
	//get reports
	$survey->getReports(<FILTER>,<PAGE>);
	$survey->getReport($id);
	//get statistics
	$survey->getStatistics();
	$survey->getStatisticsByID($question_id);
	//get campaigns
	$survey->getCampaigns();
	$survey->getCampaign($id);
	//get email messages
	$survey->getCampaign($id)->getEmailMessages();
	$survey->getCampaign($id)->getEmailMessage($email_id);
```

### Questions
To access the questions on a survey you'll need an instance of a \SurveyGizmo\Resources\Survey object.

###### Get all Survey Questions
```php
$questions = $survey->getQuestions();
```

###### Getting and Updating a Survey Question
```php
$question = $survey->getQuestion(<QUESTION question_id>);
$question->title->English = "LIBRARY TEST";
$ret = $question->save();
```

### Responses
To access the responses for a survey you'll need an instance of a \SurveyGizmo\Resources\Survey object. See filter and paging below.

###### Get all Survey Responses
```php
$responses = $survey->getResponses(<FILTER>,<OPTIONS>);
```

###### Get a Single Responses
```php
$responses = $survey->getResponse(<RESPONSE_ID);
```

###### Update a Responses
```php
$response->survey_data[$question_id]['answer'] = 'YES';
$ret = $response->save();
```


#### Filtering & Paging Objects
All fetch methods take both optional $filter and $options arguments.

###### Filtering
```php
$filter = new \SurveyGizmo\Helpers\Filter();
$filter_item = new \SurveyGizmo\Helpers\FilterItem();
$filter_item->setField('title');
$filter_item->setOperator('=');
$filter_item->setCondition('TEST from API');
$filter->addFilterItem($filter_item);
$surveys = \SurveyGizmo\Resources\Survey::fetch($filter);
```

###### Paging Collections
Sometimes you will need to page through collections of objects.  To accommodate this use the optional $options argument on any fetch method;
```php
$options = array( 'page' => 3, 'limit' => 100 );
$surveys = \SurveyGizmo\Resources\Survey::fetch($filter,$options);
```

### ERROR Message & Responses
In the case of an error we will return the following responses and status codes:
```
Method not implemented (404)
Method not supported (405)
Not Authorized (401)
```

### Simple API request
To perform a API call without going through a specific resource class, use \SurveyGizmo\ApiRequest::call.
```php
$response = \SurveyGizmo\ApiRequest::call('contactlist', null, null, null);
```

## Dependencies
```
PHP 5.3+
CURL
Active SurveyGizmo Account
Imagination, Determination and Common Sense!
```

## Installation
1. Download the Library and add it to your project.
2. Include the SurveyGizmoAutoLoader.php file
```php
require_once "<LIBRARY_PATH>/SurveyGizmoAutoLoader.php";
```
3. Authenticate using your SurveyGizmo [API Key and Secret](https://apihelp.surveygizmo.com/help/article/link/authentication).
```php

try {
	\SurveyGizmo\SurveyGizmoAPI::auth("<YOUR API_KEY>", "<YOUR API_SECRET>");
} catch (Exception $e) {
	die("Error Authenticating");
}
```

## Automated Installation
This library is now available in [packagist](https://packagist.org/), and you can include [surveygizmo/surveygizmo-api](https://packagist.org/packages/surveygizmo/surveygizmo-api) in your composer files to autoload it.

## Code Samples
Please refer to the [Samples folder](https://github.com/SurveyGizmoOfficial/PHP_SurveyGizmo_Api/tree/master/Samples) for more thorough example use cases.

To use these samples, copy the example file and then supply your own credentials:
```bash
$ cp Samples/.credentials.example Samples/.credentials
$ vi Samples/.credentials # then supply your credentials accordingly
$ cd Samples
$ php new_survey.php # run once prior to running manipulate_survey.php
```

## API Reference
This Library uses the version 5 SurveyGizmo API, please refer to our [API Documentation](https://apihelp.surveygizmo.com/help/version-5) for more information.

## Tests
Unit tests are included under the /Tests directory.  They can be run by calling PHPUnit within the Tests folder:
```bash
$ phpunit
```

## Contributors
The library was developed and is maintained by the [SurveyGizmo](http://www.surveygizmo.com) Product Development Team.

## License
This project is licensed under the terms of the MIT license.
