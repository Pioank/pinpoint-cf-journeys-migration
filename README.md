# Automating the Amazon Pinpoint Journey copying

## Description

Amazon Pinpoint Campaigns, Journeys and Segments are Project specific resource and cannot be copied between Projects. This can become a blocker when Pinpoint is used from multiple teams who would like to share existing resources or when a migration between projects or AWS Regions is required. 

Amazon Pinpoint users will need to re-create any asset they want to have in their new Amazon Pinpoint project, which is a manual tasks that takes time and it is prone to human error.

## Solution

![solution_process](https://github.com/Pioank/pinpoint-cf-journeys-migration/blob/main/CF-Journey-Copy-Process.PNG)

The solution presented in this repository, utilizes AWS CloudFormation that executes an AWS Lambda function upon deployment and copies the specified Journeys from one Amazon Pinpoint Project to another. The solution can copy journeys between AWS regions and have them created either in an existing Amazon Pinpoint Project or a new one.

**Key features:**
1. Programmatically obtain existing Journeys
3. Cross AWS Region copying
4. Creation of new Amazon Pinpoint project to paste the Journeys if you don't have one
5. Deletion of all Journeys created when deleting the CloudFormation stack

**IMPROTANT**: The solution will reset the **Starting** and **End** dates of the copied Journeys. This is done because these dates might be in the past, something that isn't allowed to have when creating a Campaign or Journey in Pinpoint. Also the **Status** of all newly created Journeys is updated to **DRAFT**, which means that you will need to publish them.

## Implementation

1. Navigate to the AWS CloudFormation console to the AWS Region that you want to paste the copied Journeys
2. Create a Stack from New Resources and select the [AWS CloudFormation template](https://github.com/Pioank/pinpoint-cf-journeys-migration/blob/main/CF-Pinpoint-Journeys-Migration.yaml) from this repository
3. Fill the template parameters as shown below:
    1. **Stack name**: Provide a name for your AWS CloudFormation stack
    2. **AWSRegionFrom**: Select from the list the AWS Region where you want to copy the existing Pinpoint journeys from
    3. **PinpointProjectIdFrom**: Provide the Amazon Pinpoint Project Id for the project that hosts the Pinpoint journeys
    4. **PinpointJourneyIds**: Type the Pinpoint Journey Ids that you want to copy separated by comma ","
    5. **PinpointProjectId**: Type the Pinpoint Project Id if you already have one where you want the Journeys to be pasted otherwise leave it empty
    6. **NewPInpointProjectName**: If you don't have an existing Pinpoint Project then type a name to create one
4. Create the Stack
