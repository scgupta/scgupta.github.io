---
title: "Architecting High-Throughput Low-Latency Big Data Pipeline on Cloud for Analytics and Machine Learning"
excerpt: "Scalable and efficient data pipelines are as important for the success of analytics, data science, and machine learning as reliable supply lines are for winning a war."
image: "https://2.bp.blogspot.com/-HfCovz_p7cw/Xl0xoGc6cYI/AAAAAAAAToc/kq0678maYuggObFMNvLgyM_LzULDxK3nACKgBGAsYHg/s1600/BigDataPipeline-Architecture-scgupta.png"
image_thumbnail: "https://2.bp.blogspot.com/-HfCovz_p7cw/Xl0xoGc6cYI/AAAAAAAAToc/kq0678maYuggObFMNvLgyM_LzULDxK3nACKgBGAsYHg/s320/BigDataPipeline-Architecture-scgupta.png"
tags:
  - "machine-learning"
  - "data-science"
  - "big-data"
  - "analytics"
---

For deploying analytics, data science, and machine learning (ML) applications in the real-world that consume a big amount of data, tuning analytics and training the models is only around 25% of the work. Approximately 50% of the effort goes into making data ready for analytics and ML. Remaining 25% effort goes into making insights and model inferences easily consumable at scale. The data pipeline puts all of it together. It is the railroad on which heavy and marvellous wagons of ML run. Long term success hinges critically on getting the data pipeline right.

In this article, you will examine desired characteristics from the engineering **perspective**, learn about the components of a data **pipeline**, architecture **possibilities** for building a scalable and efficient pipeline on cloud (including serverless on AWS, Azure and Google clouds), and tips on various dimensions to consider while deploying in **production**.

--

## Perspective

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-BYfJjFTMFMI/Xl0xoPp2UMI/AAAAAAAAToc/YOOZx0JxBaMnZ0epkmuE2GtX51gfr37cQCKgBGAsYHg/s1600/BigDataPipeline-Perspective-scgupta.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Perspective: View depends on the vantage point.</figcaption>
</figure>

From the data **science** perspective, we focus on finding the *most robust* and computationally *least expensive* model for a given problem using available data.

From the **engineering** perspective, we focus on building things that *others* can *depend on*; innovating either by building *new things* or finding *better ways* to build existing things,  that function 24x7 without much human intervention.

From the **business** perspective, we focus on *delivering value* to customers, science and engineering are means to that end.

The complete reality can be constructed only by grasping different perspectives from various vantage points. In this article, we will explore the engineering perspective from the vantage point of ingesting a big amount of data needed in ML applications. Desired engineering characteristics of a data pipeline are:

- **Accessibility:** data being easily accessible for hypothesis evaluation and model experimentation, preferably through a query language.
- **Scalability:** the ability to scale as the amount of ingested data increases.
- **Efficiency:** data and machine learning results being ready within specified latency.
- **Monitoring:** automatic alerts about the health of the data and the pipeline.

---

## Pipeline

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-KLWSU2riJLY/Xl0xoI1x3nI/AAAAAAAAToc/AcFrTLPAc6otY3lqiU8nqW3GC9T4n4_VgCKgBGAsYHg/s1600/BigDataPipeline-Pipeline-scgupta.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Pipeline: Well oiled data pipeline is a must for the success of machine learning.</figcaption>
</figure>

Value of data is unlocked only after it is transformed into actionable insight, and when that insight is promptly delivered. Otherwise, it is just some bytes wasting storage.

The **data pipeline** stitches together the end-to-end operation from collecting data and transforming it into insights or training a model, to delivering insights or applying the model whenever and wherever the action needs to be taken to achieve the business goal.

> Data is the new oil. It's valuable, but if unrefined it cannot really be used. It has to be changed into gas, plastic, chemicals, etc. to create a valuable entity that drives profitable activity; so must data be broken down, analyzed for it to have value. -- Clive Humby, UK Mathematician and architect of Tesco's Clubcard

Data pipeline has five stages grouped into three heads:
- **Data Engineering:** collection, ingestion, preparation (~50% effort)
- **Machine Learning:** computation (~25% effort)
- **Delivery:** presentation (~25% effort)

**Collection:** Data sources (mobile apps, websites, web apps, microservices, IoT devices etc.) are instrumented to collect relevant data.

**Ingestion:** The instrumentation pumps the data into various kinds of inlet points (HTTP, MQTT, message queue etc.). There can also be jobs to import data from services like Google Analytics and Mixpanel. The data can be in two forms: blobs and streams. All this data gets collected into *Data Lake*.

**Preparation:** It is the extract, transform, load (ETL) operation to cleanse, conform, shape, transform, and catalogue the data blobs and streams in the data lake; essentially making the data ready-to-consume for ML and store it in *Data Warehouse*.

**Computation:** This is where data science and machine learning happens. Computation can be a combination of batch and stream processing. Models and insights (both structured data and streams) are stored back in the warehouse.

**Presentation:** The insights are delivered through dashboards, emails, SMS, push notification, and microservices. The ML model inference is exposed as microservices.

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/-8ym2pLKA2J4/Xl0xoA6i0bI/AAAAAAAAToc/-ZjBwd9KVUoQNFBjs3WjO_3tc1FY4VD3gCKgBGAsYHg/s1600/BigDataPipeline-Stages-scgupta.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Stages in a data pipeline</figcaption>
</figure>

### Data Lake vs. Data Warehouse

[Data Lake](https://en.wikipedia.org/wiki/Data_lake){:target="_blank" rel="nofollow"} contains all data in its natural/raw form as it was received usually in blobs or files. [Data Warehouse](https://en.wikipedia.org/wiki/Data_warehouse){:target="_blank" rel="nofollow"} stores cleaned and transformed data along with catalogue and schema. The data in both the lake and warehouse can be structured (relational), semi-structured, binary, and even real-time event streams.

It is a matter of choice whether the lake and the warehouse are kept physically in different stores, or the warehouse is materialised through some kind of interface (e.g. [Hive](https://hive.apache.org/){:target="_blank" rel="nofollow"} queries) over the lake. The choice is driven by speed requirements and cost constraints.

> No matter which approach is followed, it is important to retain the raw data for audit, testing and debugging purposes.

### Exploratory Data Analysis (EDA)

Exploratory Data Analysis is to analyze and visualize data sets and formulate hypotheses. It may expose holes in the data, lead to new data collection and experimentation, eventually to testing the hypothesis.

You can think of it as small scale machine learning experiments to zero in on a small set of promising models, which are compared and tuned on the full data set.

Having a well maintained Data Warehouse with catalogues and schema, accessibly through a query language (instead of needing to write programs) facilitates speedy EDA.

---

## Possibilities

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/--pmqYtRaR1Q/Xl0xoGFBCJI/AAAAAAAAToc/OiGOYklGm_w8eT11u6n-t2OwgK5ZqS0xQCKgBGAsYHg/s1600/BigDataPipeline-Possibilities-scgupta.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">Possibilities: Architecture is a trade-off between performance and cost. There are six options sown in this image to make a triangular tent shape. From top left to bottom right, the amount of glue needed to make the tent decreases. Which one will you choose to do in prod? Notice that the base of the triangle is smaller than the other side, and the light blue piece is a rectangle and not a square.</figcaption>
</figure>

Data pipeline, lake, and warehouse are not something new. These have existed for quite long to serve data analytics through batch programs, SQL, or even Excel sheets. What has changed is the availability of big data that facilitates machine learning, and the increasing importance of real-time applications.

There are several architectures choices offering different performance and cost tradeoffs just like options shown in the accompanying image. The most optimal mathematical option may not necessarily be the most suitable solution for production. It will depend on what are your requirements:

- Do you need real-time insights or model updates?
- What is the staleness tolerance of your application?
- What are the cost constraints?

You have to combine batch and stream processing in a Lambda architecture to match your requirement of throughput and latency consisting of three layers:

- **Batch Layer:** high throughput, comprehensive, economical, but high latency map-reduce batch processing.
- **Speed Layer:** low latency real-time stream processing overcoming latencies of batch processing, but also costlier and may hit memory limitation for a very high volume of data.
- **Serving Layer:** The output from high throughput batch processing, when ready, is merged with the output of the stream processing to provide comprehensive results in the form of pre-computed views or ad-hoc queries.

The underlying assumption in the lambda architecture is that the source data model is append-only, i.e. ingested events are timestamped and appended to existing events, and do not overwrite them. Therefore, urgently needed insights can be implemented through stream processing.

### Architecture: Your choice of the stack on the cloud

The figure above shows one possible architecture using open source technologies to materialise all stages of the pipeline. The preparation and computation stages are quite often merged to optimise compute operations.

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-HfCovz_p7cw/Xl0xoGc6cYI/AAAAAAAAToc/kq0678maYuggObFMNvLgyM_LzULDxK3nACKgBGAsYHg/s1600/BigDataPipeline-Architecture-scgupta.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">An architecture of the data pipeline using open source technologies</figcaption>
</figure>

Key components of the architecture are the following:
- **HTTP / MQTT Endpoints** for ingesting data, and also for serving the results. There is an abundance of choices of frameworks and technologies for this.
- **Pub/Sub Message Queue** for ingesting high-volume streaming data. Kafka is currently the de-facto choice.  It is battle-proven to scale to high event ingestion rate.
- **Low-Cost High-Volume Data Store** for data lake (and data warehouse), Hadoop HDFS or cloud blob storage like AWS S3.
- **Query and Catalog Infrastructure** for converting a data lake to a data warehouse, Apache Hive is a popular query language choice.
- **Map-Reduce Batch Compute** engine for high throughput processing, e.g. Hadoop Map-Reduce, Apache Spark.
- **Stream Compute** for latency-sensitive processing, e.g. Apache Storm, Apache Flink. Apache Beam is an emerging choice for writing compute data-flow, and can be deployed on a Spark batch runner or Flink stream runner.
- **Machine Learning Frameworks** for data science and ML. Scikit-Learn, TensorFlow, and PyTorch are a popular choice for implementing machine learning.
- **Low-Latency Data Stores** for storing the results. There are too many well-established choices of data stores depending on data type, performance, scale and cost to cover here.
- **Deployment** orchestration options are Hadoop YARN, Kubernetes / Kubeflow.

### Architecture: Serverless

With the advent of [serverless computing](https://medium.com/r/?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FServerless_computing){:target="_blank" rel="nofollow"}, it is possible to start quickly by avoiding DevOps needed for resource allocation for managing scale. Collection happens on edge devices, so nothing changes for that stage. The rest of the stages can be replaced with their serverless counterparts from a cloud service provider.

Possible serverless architectures of data pipeline on Amazon Web Services, Microsoft Azure, and Google Cloud are shown below.

<figure class="aligncenter">
  <img src="https://4.bp.blogspot.com/-vJGU8DM2YHI/Xl0xoOPE16I/AAAAAAAAToc/8XPcJQie34oS6zWln9J1McBrodmMyofkQCKgBGAsYHg/s1600/BigDataPipeline-AWS-scgupta.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless data pipeline architecture on Amazon Web Services (AWS)</figcaption>
</figure>

<figure class="aligncenter">
  <img src="https://4.bp.blogspot.com/-Hi5APJVldL0/Xl0xoJqov0I/AAAAAAAAToc/BhaHt6glkUEW8w_YKddlkWbeOi6BHsOhgCKgBGAsYHg/s1600/BigDataPipeline-Azure-scgupta.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless data pipeline architecture on Microsoft Azure</figcaption>
</figure>

<figure class="aligncenter">
  <img src="https://4.bp.blogspot.com/-DuGjMeNxyLM/Xl0xoF59yxI/AAAAAAAAToc/zwMnNUUwTakBm_ZwvafsXLUtb2Lh-2XZACKgBGAsYHg/s1600/BigDataPipeline-GC-scgupta.png" class="framedimg" width="100%" >
  <figcaption class="aligncenter">Serverless data pipeline architecture on Google Cloud</figcaption>
</figure>

---

## Production

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-x5lX6wVZjU0/Xl0xoMXKIdI/AAAAAAAAToc/xvRf88dpfPsTqv3NgGX2N3TMt60AuAyjACKgBGAsYHg/s1600/BigDataPipeline-Production-scgupta.png" class="framedimg" width="50%" >
  <figcaption class="aligncenter">In production: simplicity quite often trumps cleverness. You may notice that the option chosen to make the triangular tent shape is not the one needing the least amount of glue. How the needed parts will be produced, and the simplicity of the overall operation is important for reducing the potential for errors.</figcaption>
</figure>

Production can be the graveyard of un-operationalized analytics and machine learning. If you do not invest in 24x7 monitoring of the health of the pipeline that raises alerts whenever some trend thresholds are breached, it may become defunct without anyone noticing.

> Be mindful that engineering and OpEx are not the only costs. Also consider time, opportunity, and stress costs while designing and prioritising.

Operationalising a data pipeline can be tricky. Here are some tips that I have learned the hard way:

-  **Scale Data Engineering before scaling Data Science team.** ML wagons can't run without first laying railroads.
- **Be industrious in clean data warehousing.** ML is only as good as data. Be disciplined in defining the schema of the data being collected, cataloguing it. In absence of that, do not be surprised by how much data rots for perpetuity in storage as mere bytes.
- **Start simple.** Start with serverless, with as few pieces as you can manage with. Move to a full-blown pipeline, or your own deployment, only when RoI is justifiable. I suggest bootstrapping with minimal investment in the computation stage. Go even "compute-less" by implementing it by scheduling a bunch of queries and cloud functions. That will get the whole pipeline ready faster, and give you ample time to focus on getting your data strategy in place, along with needed schemas and catalogues.
- **Build only after careful evaluation.** What are the business goals? What levers do you have to affect the outcome? What insights will be actionable? Collect data and build ML based on that.

--

## Summary

Key takeaways are:
- Tuning analytics and machine learning models constitutes only 25% effort
Invest in data pipeline early because analytics and ML are only as good as data
Ensure easily accessible data for exploratory work
Start from business goals, and seek actionable insights

I hope you found this article useful. What tips and tricks you got? Do share in comments.




---



[text](https){:target="_blank" rel="nofollow"}

![](){: width="100%" class="framedimg"}

<figure class="aligncenter">
  <img src="" class="framedimg" >
  <figcaption class="aligncenter"></figcaption>
</figure>
