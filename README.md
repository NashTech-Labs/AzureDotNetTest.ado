# AzureDotNetTest.ado
use the .NET Core task to run unit tests by using testing frameworks like MSTest, xUnit, and NUnit. Test results get automatically published to the service. These results can be used for troubleshooting failed tests and test-timing analysis.

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| dotNetTestProjectFilePath | Path to project(s) | String |  |  | Optional | Specifies Path to project(s) |
| buildConfiguration | Configuration | String | release | test/release | Required | Specifies Configuration to pack |
| workingDirectory | Working directory | String |  |  | Optional | Specifies Working directory |
| publishTestResults | Publish test results and code coverage | boolean | True | True/False | Optional | Use when command = test. Default value: true |
| testRunTitle | Test run title | String |  |  | Optional | Use when command = test. Test run title. |
| testResultsFormat | Test result format | String | JUnit |  | Required | Allowed values: JUnit, NUnit, VSTest, XUnit, CTest |
| testResultsFiles | Test results files | String | '**/TEST-*.xml' |  | Required | Test results files. Default: **/TEST-*.xml |

These parameters provide multiple use case options for the dotnet templates pipeline, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/AzureDotNetTest.ado
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
- ${{ if parameters.logger }}:
  - template: templates/runDotNetUnitTest.yaml
    parameters:
      testRunner: ${{ parameters.testRunner }}
      testResultsFiles: ${{ parameters.testResultsFiles }}     
- ${{ else}}:
  - template: templates/runDotNetUnitTest.yaml
    parameters:
      dotNetTestProjectFilePath: '${{ parameters.dotNetTestProjectFilePath }}' 
      buildConfiguration: '${{ parameters.buildConfiguration }}'
      publishTestResults: '${{ parameters.publishTestResults }}'
      testRunTitle: '${{ parameters.testRunTitle }}'
      workingDirectory: '${{ parameters.workingDirectory }}'

  ```
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.
