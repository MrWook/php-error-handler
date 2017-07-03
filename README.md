# mw-php-error

mw-php-error is a PHP function with the aim to catch almost every PHP error, like Execution timeout, overflows and syntax error and many more.
But this isn't some weird magic so it can't catch the error if the Server goes down.

## Requirements (tested in)
- PHP (v5.5.6)

## Install

Download the file and include it into your application

## Usage

Include the script into your php script and initialize the error handler
```php
include('mw-php-error.php')
error_init(`Log function`, `Memory`, `Reporting`, `Show error`, `Error level`);
```
    
## error_init Parameter

Name                    | Type      | Default      | Description
----------------------- | --------- | ------------ | ------------
Log function            | string    | No default   | Set the log function name which should be used when an error occurred
Memory                  | int       | 524288       | set the internal error memory save for php overflows if the $log_object or the $log_info is to big it need to be increased
Reporting               | boolean   | true         | set if the custom handling should be activated need to be true to log the error's
Show error              | boolean   | false        | set if the PHP errors should be shown on the website
Error level             | int       | E_ALL        | set the error level type

## log function Parameter

Name       | Type      | Default      | Description
-----------| --------- | ------------ | ------------
Type       | string    | Error        | Just the status, this doesn't change
log_object | array     | 524288       | The log object can be used to store important information. It is a global variable. If an error occure everything in $log_object will be transmitted to the logging function.
dataset    | array     | true         | In the dataset are the information about the error. Also in the dataset are information from $log_info to store information about the process where the error occure. $log_info is a global variable

## Log Example
```php
include('mw-php-error.php')
error_init('logging');
 
function logging($type, $log_object, $dataset){
    //write into log file
    $myfile = fopen("log.txt", "a");
    $text = json_encode([
        "date" => date("Y-m-d H:i:s"),
        "log_object" => $log_object,
        "dataset" => $dataset
    ]);
    fwrite($myfile, $text);
    fclose($myfile);
    
    //response with a clean text
    $response = [
        'type' => 'warning',
    	'text' => 'An error occured'
    ];
    echo json_encode($response);
}
```