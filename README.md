# Authomate Performance test using Taurus with Jmeter

leverage taurus's capabilities to adopt an existed open source functional and performance testing tools to automate and use them in a way to build contious integration process using jenkins. Taurus relies on JMeter, Gatling, Locust.io, Grinder and Selenium WebDriver as its underlying tools. Free and open source under Apache 2.0 License.
for example ,configuration options and reports which JMeter does not offer by default and also,easly to debugging errors,maintain and simple to understand.

## Getting Started
This instruction leads you how to install taurus on mac platform ,clone and execute jmeter performance testing plan that using taurua yaml command to upload files using REST API

### Prerequisites
Install , jemeter, python 2.7 or higher and latest Taurus on your mac

### Installing

```
brew install python
```
then install taurus

```
sudo pip install bzt
```
feature upgrade taurus to the latest release
```
sudo pip uninstall bzt && sudo pip install bzt
```
Note : If there is no JMeter installed at the configured path, Taurus will attempt to install the latest JMeter and associated plugins into this location (by default this is: ~/.bzt/jmeter-taurus/bin/jmeter). You can change this setting to your preferred JMeter location (consider putting it into the ~/.bzt-rc file).

## Explain taurus script 
Here we use Taurus’ simple configuration syntax to create a test scenario as a YAML file.Create a file named uploadFileTest.yml with following contents:



Example:

```
execution:
- concurrency: 10
  hold-for: 1m
  ramp-up: 50s
  scenario: File Upload using REST API
  
  scenarios:
  File Upload using REST API:
    
    requests:
    - assert:
      - assume-success: false
        contains:
        - '"succeeded"'
        - '"failed":0'
        not: false
        regexp: false
        subject: body
      follow-redirects: true
      label: 'uploading '
      method: POST
      upload-files:
      - mime-type: text/csv
        param: fileData
        path: MiscTest.csv
      headers:
        Authorization: Basic ${__base64Encode(${__P(user-auth)})}
      url: http://${host}:${port}/zoomdata/api/upload/${sourceId}?accountId=${accountId}
    store-cache: false
    store-cookie: false
    use-dns-cache-mgr: false
  ```
  Define user valiable as part of the main script
  
  ```
  variables:
      accountId: 5a2ef7a2e4b043bb0659cddf
      host: localhost
      port: '8080'
      sourceId: 5a4588ffe4b043bb5a947999
 ```
 Include extra config file : Taurus will merge into resulting configuration any included-configs from the list
 
 ```
      included-configs:  # it must be a list of string values
      - config.yml  # to add local file just set its path
```
Response assertions for text contain "succeeded" and "Failed = 0"

```
- assert:
      - assume-success: false
        contains:
        - '"succeeded"'
        - '"failed":0'
        not: false
        regexp: false
```
Adding JMeter Properties
JMeter’s default configuration (see jmeter.bat for Windows or jmeter for non-Windows systems scripts) assumes a heap space of 512 megabytes only. This is actually pretty low for this experiment and allocated 4GB RAM space for jmeter in modules section of this script.

 ```
 modules:
  jmeter:
    memory-xmx: 12G  # allow JMeter to use up to 4G of memory
 ```
### Clone and run this project

git clone (https://github.com/jemalft/performance-taurus.git/)

Modify url,Authorization and file path location
### Run
```
bzt uploadFileTest.yml
```
### Trigger BlazeMeter Reporting Service
Another advantage of running a JMeter script with Taurus is that you get access to BlazeMeter’s reporting service by adding a simple command like --report when you execute the yml file.

```
bzt uploadFileTest.yml --report
```

### Expected Result 
* Live taurus report.
* BlazeMeter report will be triggered on new browser session.
* Test result Artifacts in the project dir.

  
