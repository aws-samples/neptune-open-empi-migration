# Implementing OpenEMPI patient matching in Amazon Neptune

This repository has a code sample accompanying our post on the AWS Database Blog: *Implementing OpenEMPI patient matching in Amazon Neptune*.

In the post, we discuss how to adopt the Open Enterprise Master Patient Index (OpenEMPI) architecture in Amazon Neptune, a managed graph database service. OpenEMPI is an architecture for a central repository of patients across facilities. It is designed to accommodate incomplete or inaccurate patient data and avoid duplicate records, a common challenge with patient data. As we show, OpenEMPI’s patient model is a good fit for representation in a graph database such as Neptune. Representing that data in a graph enables us to better match patients and detect patient relationships.

See <https://www.openhealthnews.com/content/openempi> for more on OpenEMPI.

Our overall architecture is the following:

<img width="373" alt="image" src="https://github.com/aws-samples/neptune-open-empi-migration/assets/9324867/b22884a9-362d-47c1-9132-e8a2ecee5063">

In this demo, we show a subset of the overall architecture. Our focus is to demonstate how to export OpenEMPI patient data from OrientDB, load that data into an Amazon Neptune database, and then query that data using the Gremlin query language. 

OrientDB manages three sets of data: Patient, Person, Provider. We consider only Patient in this demo. OrientDB can export its data to JSON files. These files can be very large. We use a Converter tool, written in Node.js, to convert JSON to CSV. We then bulk -load that data to Neptune.

<img width="382" alt="image" src="https://github.com/aws-samples/neptune-open-empi-migration/assets/9324867/4a81e83a-ff49-48fc-914b-fbfa7d7a441e">

Once the data is in Neptune, we can query it.  Here is the graph data model we use.

<img width="411" alt="image" src="https://github.com/aws-samples/neptune-open-empi-migration/assets/9324867/b8a1dc06-25d4-465d-8a86-9d09ea4f9771">

We drive the end-to-end flow using a notebook. 

Refer to the blog post for a more detailed discussion. 

## Setup
To setup this demo in your own AWS account, first clone this repo locally. Alternatively, download a copy of <cfn/PatientGraphStack.yaml>. Then follow these steps.

1. On the AWS CloudFormation console, choose *Create stack*.
2. Choose *With new resources (standard)*.
3. Select *Upload a template file*.  
4. Choose *Choose file* to upload the local copy of the template that you downloaded. The name of the file is PatientGraphStack.yml.
5. Choose *Next*.
6. Enter a stack name of your choosing. 
7. You may keep default values in the *Parameters* section.
8. Choose *Next*.
9. Continue through the remaining sections.
10. Read and select the check boxes in the *Capabilities* section.
11. Choose *Create stack*.
12. When the stack is complete, navigate to the *Outputs* section and follow the link for the output NeptuneSagemakerNotebook. 

This opens a notebook that you use in the remaining steps.

It sets up an Amazon Neptune cluster,
