---
title: "Scalable Efficient Big Data Pipeline Architecture"
excerpt: "Scalable and efficient data pipelines are as important for the success of analytics, data science, and machine learning as reliable supply lines are for winning a war."
image: "https://2.bp.blogspot.com/-LhyoCiRyjxc/Xl_nyu6KZEI/AAAAAAAATrs/2tGY-fRMaAAAHQSAKBYmZecDhSJDnQpCwCKgBGAsYHg/s1600/BigDataPipeline-Architecture.png"
image_thumbnail: "https://2.bp.blogspot.com/-LhyoCiRyjxc/Xl_nyu6KZEI/AAAAAAAATrs/2tGY-fRMaAAAHQSAKBYmZecDhSJDnQpCwCKgBGAsYHg/s320/BigDataPipeline-Architecture.png"
tags:
  - "machine-learning"
  - "data-science"
  - "big-data"
  - "analytics"
---

For deploying big-data analytics, data science, and machine learning (ML) applications in real-world, analytics-tuning and model-training is only around 25% of the work. Approximately 50% of the effort goes into making data ready for analytics and ML. The remaining 25% effort goes into making insights and model inferences easily consumable at scale. The big data pipeline puts it all together. It is the railroad on which heavy and marvelous wagons of ML run. Long term success depends on getting the data pipeline right.

This article gives an introduction to the data pipeline and an overview of big data architecture alternatives through the following four sections:

- **Perspective**: By understanding the perspectives of all stakeholders, you can enhance the impact of your work. This section explains various perspectives and examines the desired engineering characteristics of a data pipeline.
- **Pipeline**: In this section, you will learn about the conceptual stages of a big data pipeline passing through the **data lake** and the **data warehouse**.
- **Possibilities**: In this section, you will learn about the **lambda architecture** for balancing scale and speed, and **technology choices** for the key components of the big data architecture. You will also get a glimpse of **serverless** pipelines on **AWS**, **Azure**, and **Google Cloud**.
- **Production**: This section offers tips for your big data pipeline deployment to be successful in production.

---

## Perspective

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-HiKuS7HoEcA/Xl_nymu3PAI/AAAAAAAATrs/WTH4QHNUihARj5H_3r9t_HBYDyWcCS3LwCKgBGAsYHg/s1600/BigDataPipeline-Perspective.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Perspective: View depends on the vantage point.</figcaption>
</figure>

There are three stakeholders involved in building data analytics or machine learning applications: data scientists, engineers, and business managers.

From the data **science** perspective, the aim is to ***find*** the *most robust* and computationally *least expensive* model for a given problem using available data.

From the **engineering** perspective, the aim is to ***build*** things that *others* can *depend on*; to innovate either by building *new things* or finding *better* ways to build existing things that function 24x7 without much human intervention.

From the **business** perspective, the aim is to ***deliver*** value to customers; science and engineering are means to that end.

In this article, we will focus on the engineering perspective, and specifically the aspect of processing huge amount of data needed in ML applications, while keeping other perspectives in mind. Desired engineering characteristics of a data pipeline are:

- **Accessibility**: data being easily accessible to data scientists for hypothesis evaluation and model experimentation, preferably through a query language.
- **Scalability**: the ability to scale as the amount of ingested data increases, while keeping the cost low.
- **Efficiency**: data and machine learning results being ready within the specified latency to meet the business objectives.
- **Monitoring**: automatic alerts about the health of the data and the pipeline, needed for proactive response to potential business risks.

---

## Pipeline

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/-Jtf8ejaeJ3I/Xl_nyhLOIHI/AAAAAAAATrs/H6gMYdTr-YcZMmp7QnZ9U03Z6wA6a8HcQCKgBGAsYHg/s1600/BigDataPipeline-Pipeline.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Pipeline: Well oiled big data pipeline is a must for the success of machine learning.</figcaption>
</figure>

The value of data is unlocked only after it is transformed into actionable insight, and when that insight is promptly delivered.

A **data pipeline** stitches together the end-to-end operation consisting of *collecting* the data, *transforming* it into insights, *training* a model, *delivering* insights, *applying* the model whenever and wherever the action needs to be taken to achieve the business goal.

> Data is the new oil. It's valuable, but if unrefined it cannot really be used. It has to be changed into gas, plastic, chemicals, etc. to create a valuable entity that drives profitable activity; so must data be broken down, analyzed for it to have value.
> - Clive Humby, UK Mathematician and architect of Tesco's Clubcard

A data pipeline has five stages grouped into three heads:

- **Data Engineering**: collection, ingestion, preparation (~50% effort)
- **Analytics / Machine Learning**: computation (~25% effort)
- **Delivery**: presentation (~25% effort)

**Collection**: Data sources (mobile apps, websites, web apps, microservices, IoT devices etc.) are instrumented to collect relevant data.

**Ingestion**: The instrumented sources pump the data into various inlet points (HTTP, MQTT, message queue etc.). There can also be jobs to import data from services like Google Analytics. The data can be in two forms: blobs and streams. All this data gets collected into a **Data Lake**.

**Preparation**: It is the extract, transform, load (ETL) operation to cleanse, conform, shape, transform, and catalog the data blobs and streams in the data lake; making the data ready-to-consume for ML and store it in a **Data Warehouse**.

**Computation**: This is where analytics, data science and machine learning happen. Computation can be a combination of batch and stream processing. Models and insights (both structured data and streams) are stored back in the **Data Warehouse**.

**Presentation**: The insights are delivered through dashboards, emails, SMSs, push notifications, and microservices. The ML model inferences are exposed as microservices.

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/-zX9Z5_aQiPU/Xl_nysJ6wvI/AAAAAAAATrs/we75i54R78oY-UjR_0FSNfo0zvTSckldgCKgBGAsYHg/s1600/BigDataPipeline-Conceptual.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Stages in a big data pipeline</figcaption>
</figure>

### Data Lake vs. Data Warehouse

The [Data Lake](https://en.wikipedia.org/wiki/Data_lake){:target="_blank" rel="noopener nofollow"} contains all data in its natural/raw form as it was received usually in blobs or files. The [Data Warehouse](https://en.wikipedia.org/wiki/Data_warehouse){:target="_blank" rel="noopener nofollow"} stores cleaned and transformed data along with catalog and schema. The data in the lake and the warehouse can be of various types: structured (relational), semi-structured, binary, and real-time event streams.

It is a matter of choice whether the lake and the warehouse are kept physically in different stores, or the warehouse is materialized through some kind of interface (e.g. [Hive](https://hive.apache.org/){:target="_blank" rel="noopener nofollow"} queries) over the lake. The choice is driven by speed requirements and cost constraints.

> No matter which approach is followed, it is important to retain the raw data for audit, testing and debugging purposes.

### Exploratory Data Analysis (EDA)

The role of Exploratory Data Analysis (EDA) is to analyze and visualize data sets, and formulate hypotheses. It may expose gaps in the collected data, lead to new data collection and experiments, and verify a hypothesis.

You can think of them as small scale ML experiments to zero in on a small set of promising models, which are compared and tuned on the full data set.

Having a well maintained Data Warehouse with catalogs, schema, and accessibility through a query language (instead of needing to write programs) facilitates speedy EDA.

---

## Possibilities

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-RnQO0Avxfk8/Xl_nynDPlkI/AAAAAAAATrs/VPbfmv-wW64I_iECFYMMNXfBJGxbVdX2gCKgBGAsYHg/s1600/BigDataPipeline-Possibilities.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Possibilities: Architecture is a trade-off between performance and cost. There are six options sown in this image to make a triangular tent shape. From top left to bottom right, the amount of glue needed to make the tent decreases. Which one will you choose to do in prod? Notice that the base of the triangle is smaller than the other side, and the light blue piece is a rectangle and not a square. </figcaption>
</figure>

Data pipeline, data lake, and data warehouse are not new concepts. In the past, data analytics has been done using batch programs, SQL, or even Excel sheets. What has changed now is the availability of big data that facilitates machine learning, and increasing demand for real-time insights.

There are several architecture choices offering different performance and cost tradeoffs (just like options in the accompanying image). I have learned that the technically best option may not necessarily be the most suitable solution in production. You must carefully examine your requirements:

- Do you need real-time insights or model updates?
- What is the staleness tolerance of your application?
- What are the cost constraints?

Based on the answers to these questions, you have to balance the batch and the stream processing in the Lambda architecture to match your requirements of throughput and latency. Lambda architecture consists of three layers:

- **Batch Layer:** offers high throughput, comprehensive, economical map-reduce batch processing, but higher latency.
- **Speed Layer:** offers low latency real-time stream processing, but costlier and may overshoot memory limit when data volume is high.
- **Serving Layer:** The output from high throughput batch processing, when ready, is merged with the output of the stream processing to provide comprehensive results in the form of pre-computed views or ad-hoc queries.

The underlying assumption in the lambda architecture is that the source data model is append-only, i.e. ingested events are timestamped and appended to existing events, and never overwritten.

### Big Data Architecture: Your choice of the stack on the cloud

The following figure shows an architecture using open source technologies to materialize all stages of the big data pipeline. The preparation and computation stages are quite often merged to optimize compute costs.

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-LhyoCiRyjxc/Xl_nyu6KZEI/AAAAAAAATrs/2tGY-fRMaAAAHQSAKBYmZecDhSJDnQpCwCKgBGAsYHg/s1600/BigDataPipeline-Architecture.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">An architecture of the data pipeline using open source technologies</figcaption>
</figure>

Key components of the big data architecture and technology choices are the following:

- **HTTP / MQTT Endpoints** for ingesting data, and also for serving the results. There are several frameworks and technologies for this.
- **Pub/Sub Message Queue** for ingesting high-volume streaming data. [Kafka](https://kafka.apache.org/){:target="_blank" rel="noopener nofollow"} is currently the de-facto choice.  It is battle-proven to scale to high event ingestion rate.
- **Low-Cost High-Volume Data Store** for data lake (and data warehouse), [Hadoop HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html){:target="_blank" rel="noopener nofollow"} or cloud blob storage like [AWS S3](https://aws.amazon.com/s3/){:target="_blank" rel="noopener nofollow"}.
- **Query and Catalog Infrastructure** for converting a data lake into a data warehouse, Apache [Hive](https://hive.apache.org/){:target="_blank" rel="noopener nofollow"} is a popular query language choice.
- **Map-Reduce Batch Compute** engine for high throughput processing, e.g. [Hadoop Map-Reduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html){:target="_blank" rel="noopener nofollow"}, Apache [Spark](https://spark.apache.org/){:target="_blank" rel="noopener nofollow"}.
- **Stream Compute** for latency-sensitive processing, e.g. Apache [Storm](https://storm.apache.org/){:target="_blank" rel="noopener nofollow"}, Apache [Flink](https://flink.apache.org/){:target="_blank" rel="noopener nofollow"}. Apache [Beam](https://spark.apache.org/){:target="_blank" rel="noopener nofollow"} is an emerging choice for writing compute data-flow, and can be deployed on a Spark batch runner or Flink stream runner.
- **Machine Learning Frameworks** for data science and ML. [Scikit-Learn](https://scikit-learn.org/){:target="_blank" rel="noopener nofollow"}, [TensorFlow](https://www.tensorflow.org/){:target="_blank" rel="noopener nofollow"}, and [PyTorch](https://pytorch.org/){:target="_blank" rel="noopener nofollow"} are a popular choice for implementing machine learning.
- **Low-Latency Data Stores** for storing the results. There are too many well-established choices of data stores depending on data type, performance, scale and cost to cover here.
- **Deployment** orchestration options are [Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html){:target="_blank" rel="noopener nofollow"}, [Kubernetes](https://kubernetes.io){:target="_blank" rel="noopener nofollow"} / [Kubeflow](https://www.kubeflow.org){:target="_blank" rel="noopener nofollow"}.

Scale and efficiency are controlled by the following levers:

- **Throughput** depends on the **scalability** of the ingestion (i.e. **REST/MQTT** endpoints and **message queue**), data lake **storage** capacity, and **map-reduce** batch processing.
- **Latency** depends on the **efficiency** of the **message queue**, **stream compute** and **databases** used for storing computation results.

### Big Data Architecture: Serverless

With the advent of [serverless computing](https://en.wikipedia.org/wiki/Serverless_computing){:target="_blank" rel="noopener nofollow"}, it is possible to start quickly by avoiding DevOps. Various components in the architecture can be replaced by their serverless counterparts from the chosen cloud service provider.

Typical serverless architectures of big data pipelines on **Amazon Web Services**, **Microsoft Azure**, and **Google Cloud Platform (GCP)** are shown below. Each maps closely to the general big data architecture discussed in the previous section. You can use these as a reference for shortlisting technologies suitable for your needs.

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/-KJcUTrUPLXo/Xl_nysua7kI/AAAAAAAATrs/5Kmrx9DW3hsUqBL_9T9Ez2tH-V3GWAOXgCKgBGAsYHg/s1600/BigDataPipeline-AWS.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless big data pipeline architecture on Amazon Web Services (AWS)</figcaption>
</figure>

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-C7mZX5Sfqh0/Xl_nyl476tI/AAAAAAAATrs/SFPBR9HQ3BsCOeMj7RmUrCsScD7mb9RDACKgBGAsYHg/s1600/BigDataPipeline-Azure.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless big data pipeline architecture on Microsoft Azure</figcaption>
</figure>

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-GG4oe6NS_gw/Xl_nytqF00I/AAAAAAAATrs/MPwSqn5w8NA4OTp7_U5i0SRC5_7emV43ACKgBGAsYHg/s1600/BigDataPipeline-GC.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless big data pipeline architecture on Google Cloud Platform (GCP)</figcaption>
</figure>

---

## Production

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-3YdHo1CYcKo/Xl_nylyK-MI/AAAAAAAATrs/U7KZblPuP-AvWDKD3Ubbh_vzcrC1KisowCKgBGAsYHg/s1600/BigDataPipeline-Production.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">In production: simplicity quite often trumps cleverness. You may notice that the option chosen to make the triangular tent shape is not the one needing the least amount of glue. How the needed parts will be produced, and the simplicity of the overall operation is important for reducing the potential for errors. </figcaption>
</figure>

Production can be the graveyard of un-operationalized analytics and machine learning. If you do not invest in 24x7 monitoring of the health of the pipeline that raises alerts whenever some trend thresholds are breached, it may become defunct without anyone noticing.

> Be mindful that engineering and OpEx are not the only costs. While deciding architecture, consider time, opportunity, and stress costs too.

Operationalising a data pipeline can be tricky. Here are some tips that I have learned the hard way:

-  **Scale Data Engineering before scaling Data Science team.** ML wagons can’t run without first laying railroads.
- **Be industrious in clean data warehousing.** ML is only as good as data. Be disciplined in defining the schema of the data being collected, cataloging it. In absence of that, do not be surprised by how much *data* rots for perpetuity in storage as mere *bytes*.
- **Start simple.** Start with serverless, with as few pieces as you can manage with. Move to a full-blown pipeline, or your own deployment, only when RoI is justifiable. Bootstrap with minimal investment in the computation stage. Go even “compute-less” by implementing computations by scheduling a bunch of SQL queries and cloud functions. That will get the whole pipeline ready faster, and give you ample time to focus on getting your data strategy in place, along with data schemas and catalogs.
- **Build only after careful evaluation.** What are the business goals? What levers do you have to affect the business outcome? What insights will be actionable? Collect data and build ML based on that.

--

## Summary

Key takeaways are:

- Tuning analytics and machine learning models is only 25% effort.
- Invest in data pipeline early because analytics and ML are only as good as data.
- Ensure easily accessible data for exploratory work.
- Start from business goals, and seek actionable insights.

I hope you found this article useful. What tips and tricks you have for building a robust data pipeline in production? Do share in comments.
