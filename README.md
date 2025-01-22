 ![Recorder feedback logo (2)](https://github.com/user-attachments/assets/87515c0b-d0a2-4c8d-846f-b81a794a0f1f)
 # Recorder Feedback

### Quick links to other repositories:
 * Controller App: https://github.com/BiologicalRecordsCentre/recorder-feedback-controller
 * Content Generation: https://github.com/BiologicalRecordsCentre/recorder-feedback-content
 * Connectors
   * Indicia: https://github.com/BiologicalRecordsCentre/recorder-feedback-indicia-connector
 * Repository: https://github.com/BiologicalRecordsCentre/recorder-feedback-repository

## Introduction

Biological recorders contribute valuable biodiversity data; and extensive infrastructure exists to support dataflows from recorders submitting records to databases. However, we lack infrastructure dedicated to providing informative feedback to recorders in response to the data they have contributed. By developing this infrastructure, we can create a feedback loop leading to better data and more engaged data providers.

The **Recorder Feedback** system is a suite of tools designed to manage and dispatch feedback to biological recorders, providing valuable insights into the data they have submitted. It integrates several components, including user subscription management, content generation, and content dispatch, all of which are designed to help organizations efficiently communicate with biological recorders. This guide will cover the main components, how they interact, and how to set up and use the system.

The Recorder Feedback system is meant to be modular, flexible and not tied to any perticular recording plafrom (eg. iRecord, iNaturalist). 

## System Overview

In order to be able to deliver personalised, automated feedback, the Recorder Feedback system needs to meet the following needs:
1. **User Information Management and Subscription Management**: Storing, managing user details and their subscription to feedback lists.
2. **Getting Biological Records from Databases**: Pulling biological records from databases like Indicia.
3. **Content Generation**: Tools to create customized content based on user data.
4. **Content Dispatch**: Sending the generated feedback to users.

## User and subscription management

User and subscription management is done through a **Recorder Feedback Controller App**, which is a Python Flask application. The system manages user profiles and their subscriptions to feedback lists. The Controller App provides a range of API endpoints to manage users, their subscriptions, and (optionally) the dispatch of the feedback content. This was designed to be a standalone entity, rather than add-ons to perticular recording plaforms as it makes it easier to integrate with different recording platforms.

The controller app is available here: https://github.com/BiologicalRecordsCentre/recorder-feedback-controller

API documentation is here: https://github.com/BiologicalRecordsCentre/recorder-feedback-controller/wiki/Manging-user-subscriptions-from-an-external-service

### Summary:
- **Flask Application**: The core of the system, providing REST API endpoints for managing users, lists, and feedback.
- **SQLite Database**: Stores information about users, subscriptions, lists, and feedback dispatch history.
- **Endpoints**: The API allows for operations like creating users, updating profiles, managing subscriptions, and triggering content dispatch.
- **Scheduler**: The system includes a job scheduler for automating feedback dispatch based on a schedule (e.g., daily, weekly).

## Subscribing to Lists

Users can subscribe to different feedback categories based on their preferences. This functionality is not provided by the Controller App and so separate means to sign-up (via the Controller App endpoints) are required. These are envisaged to be developed as intergrations with recording platforms. Here are the connectors that have been developed so far:

| Platform | Description | Link |
| --- | --- | --- |
| Indicia | A drupal module that integrates with the Indicia recording system. It allows users to manage their feedback subscriptions from a web interface. | https://github.com/BiologicalRecordsCentre/recorder-feedback-indicia-connector |

## Generating Content

Once users have subscribed to lists, the system can generate personalized feedback based on their biological records. Content generation is managed through the **Recorder Feedback Content** project. Feedback content is generated by combining user data with the appropriate template for each feedback list. This is written in R and uses a {targets} pipeline to manage generation and R markdown for creating the feedback items. The default output is email-ready HTML but this could equally be deve 

The Recorder Feedback Content tools are available here: https://github.com/BiologicalRecordsCentre/recorder-feedback-content
The associated documentation is available here: https://biologicalrecordscentre.github.io/recorder-feedback-content

### Recorder Feedback Content:
- **GitHub Repository**: The content generation pipeline is available on GitHub, and can be forked or modified to meet specific organizational needs.
- **Pipeline**: The system uses a customizable pipeline that pulls data from the database (e.g., Indicia) and generates feedback emails tailored to each user. This can include information like new biological records submitted, trends, and summaries.
- **R Markdown**: The system uses R markdown to make it easy to design and implement recorder feedback items.

## Getting Biological Records from Databases

The feedback content relies on biological records, which are often retrieved from an external database, such as the **Indicia** platform. In addition to Indicia, other databases can be integrated via API to fetch biological records. The system can be extended to pull data from multiple sources, depending on the specific needs of the organization.

| Platform | Description | Link |
| --- | --- | --- |
| Indicia | Records can be accessed via API using elastic search, authentication required. | https://indicia-docs.readthedocs.io/en/stable/developing/rest-web-services/elasticsearch.html

Example code for getting data from Indicia databases is provided in Recorder Feedback Content: https://biologicalrecordscentre.github.io/recorder-feedback-content/#Get_Records_data_from_an_Indicia_warehouse

## Dispatching Content

Once content has been generated, the system dispatches it to users via email. There are multiple methods for dispatching feedback content:

### Dispatching from R Feedback Generator:
- The feedback generation pipeline also has an R-based approach for sending emails. See: https://biologicalrecordscentre.github.io/recorder-feedback-content
