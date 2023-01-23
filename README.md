
>This repository falls under the NIH STRIDES Initiative. STRIDES aims to harness the power of the cloud to accelerate biomedical discoveries. To learn more, visit https://cloud.nih.gov. 
>
# NIH Cloud Lab for GCP
---------------------------------

There are a lot of resources available to learn about GCP, which can be overwhelming. NIH Cloud Lab’s goal is to make cloud very easy and accessible for you, so that you can spend less time on administrative tasks and focus on your research.

Use this repository to learn about how to use GCP by exploring the linked resources and walking through the tutorials. If you are a beginner, we suggest you begin with this Jumpstart section. If you already have foundational knowledge of GCP and cloud, feel free to skip ahead to the [tutorials](/tutorials/) section for in-depth examples of how to run specific workflows such as genomic variant calling and medical image analysis.


## Overview of Page Contents

+ [Getting Started](#gs)
+ [Overview](#ov)
+ [Command Line Tools](#cli)
+ [Google Cloud Marketplace](#mark)
+ [Ingest and Store Data](#sto)
+ [Virtual Machines](#vm)
+ [Disk Images](#im)
+ [Jupyter Notebooks](#jup)
+ [Creating Conda Environments](#co)
+ [Managing Containers](#dock)
+ [Serverless Functionality](#ser)
+ [Clusters](#clu)
+ [Billing and Benchmarking](#bb)
+ [Cost Optimization](#cost)
+ [Managing Your Code](#code)
+ [Getting Support](#sup)
+ [Additional Training](#tr)

## **Getting Started** <a name="gs"></a>
You can learn a lot of what is possible on GCP in the [GCP Getting Started Page](https://cloud.google.com/getting-started). There you can find links to [documentation](https://cloud.google.com/docs) for common GCP tools and resources, and short videos on various subjects called [cloud minute](https://www.youtube.com/playlist?list=PLIivdWyY5sqIij_cgINUHZDMnGjVx3rxi). You can also view the following [Google Cloud Essentials Playlist](https://www.youtube.com/playlist?list=PLIivdWyY5sqKh1gDR0WpP9iIOY00IE0xL) or [Cloud Bytes Playlist]( https://www.youtube.com/playlist?list=PLIivdWyY5sqIQ4_5PwyyXZVdsXr3wYhip) from Google to help you get started.

Even with a wealth of resources it can be difficult to know where to start on learning how to use the cloud. To help you, we thought through some of the most common tasks you will encounter doing cloud-enabled research and gathered tutorials and guides specific to those topics. We hope the following materials are helpful as you explore migrating your research to the cloud. Please feel free to submit [issues](/issues) or even contribute to the codebase directly. If you want additional resources on bioinformatics in GCP, check out this [other repository](https://github.com/lynnlangit/gcp-for-bioinformatics) by Lynn Langit.

Before going any further, **make sure you can open your GCP project**. Follow [our guide](/docs/open_GCP_project.md) if you have any trouble.

## **Overview** <a name="ov"></a>
There are three primary ways you can run analyses using GCP: using virtual machines, Jupyter Notebook instances, and Managed services. We give a brief overview of each of these here and go into more detail in the sections below. [Virtual machines](https://cloud.google.com/compute) are like your desktop computers, but you access them through the cloud console and you get to decide what resources are on that computer such as CPU and memory. In GCP, the platform that hosts these virtual machines is called Compute Engine. You access VMs via SSH (secure remote connections), either through the console or via the command line. Jupyter Notebook instances are virtual machines with Jupyter Lab preloaded onto them. On GCP these are run through [Vertex AI](https://cloud.google.com/vertex-ai). You decide what kind of virtual machine you want to 'spin up' and then you can run Jupyter notebooks on that virtual machine. You access these notebooks through the console similar to the way you interact with Jupyter locally. Finally, Managed Services are services allow you to run things, an analysis, an app, a website, and not have to deal with your own servers (VMs). There are still servers running somewhere, you just don't have to manage them. All you have to do is call a command that runs your analysis in the background and copies the output files to a storage bucket. The most common managed serverless feature you will work with here is the [Life Sciences API](https://cloud.google.com/life-sciences/docs/reference/rest), although in the near future this functionality will migrate to [Google Batch](https://cloud.google.com/batch/docs/get-started). Typically, these workflows are run from the command line, either from a VM, Cloud Shell, or your local terminal.

## **Command Line Tools** <a name="cli"></a>
One other task that will enable all that comes below is installing and configuring the GCP SDK command line tools, which will allow you to interact with instances or Google Storage buckets from your local terminal. Command line interface (CLI) tools are those that you use directly in a terminal/shell as opposed to clicking within a graphical user interface (UI). Instructions for installing the CLI can be found [here](https://cloud.google.com/sdk/docs/install). Along the same lines, it is important to familiarize yourself with the two main CLI commands: [gcloud](https://cloud.google.com/sdk/docs/cheatsheet) and [gsutil](https://cloud.google.com/storage/docs/quickstart-gsutil). There are also other commands you may come across in some circumstances like [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/). If you have trouble installing the CLI on your local computer, you can still use the same commands from a virtual machine or from [Cloud Shell](https://cloud.google.com/shell), which is a terminal environment available to users on the GCP console.

## **Google Cloud Marketplace** <a name="mark"></a>
The [GCP Marketplace](https://console.cloud.google.com/marketplace?_ga=2.218253598.688556330.1669914945-1153994297.1666892959) is a platform similar to Amazon.com where you can search for and launch pre-configured solutions such as Machine Images. Examples of images you may launch would be those with [enhanced security](https://console.cloud.google.com/marketplace/product/cis-public/cis-centos-linux-7-level-1) or ones opimized for various tasks like machine learning (https://console.cloud.google.com/marketplace/browse?_ga=2.218253598.688556330.1669914945-1153994297.1666892959&q=machine%20learning), or [accelerated genomics](https://console.cloud.google.com/marketplace/product/nvidia-ngc-public/nvidia-clara-parabricks-new-plan). One of our tutorials showcases using a Marketplace solution called miniKF, which you can test out [here](/tutorials/notebooks/DL-gwas-gcp-example/).

## **Ingest and Store Data using Google Cloud Storage** <a name="sto"></a>
Data can be stored in two places on the cloud, either in a cloud storage bucket, which on GCP is called Google Cloud Storage (GCS), or on an instance, which usually has Elastic Block Storage. In general, you want to keep your compute and storage separate, so you should aim to storage data in GCS for access, then only copy the data you need to a particular instance to run an analysis, then copy the results back to GCS. In addition, the data on an instance is only available when the instance is running, whereas the data in GCS is always available. [Here](https://cloud.google.com/storage/docs/quickstart-console) is a great tutorial on how to use GCS and is worth going through to learn how it all works.

We also wanted to give you a few other tips that may be helpful when it comes to moving and storing data. If your end goal is to move data to a GCS bucket, you can do that using the user interface and clicking the `Upload Files` button from within a bucket, or you can use the command line by typing `gsutil cp <FILE> <gs://BUCKET>`. Of course, you need to first create the bucket, which you can do using the instructions in the tutorial linked above, by typing `gsutil mb <BUCKET_NAME>`. If you want to move a whole folder, then use the recursive flag: `gsutil cp -r <DIR> <gs://BUCKET>`. The same applies whether moving data from your local directory or from a VM. Likewise, you can move data from GCS back to your local machine or your VM with `gsutil cp <gs://BUCKET/FILE> <DESTINATION/PATH>`. To multithread a gsutil action, use the `-m` flag, for example: `gsutil -m -r <Dir> <gs://BUCKET>`. Finally, if you are trying to move data from the Short Read Archive (SRA) to an instance, or to GCS, you can use [fasterq_dump](https://github.com/glarue/fasterq_dump) from the SRA toolkit. The best way is probably to install on an instance, then copy the data to local storage on your instance, then optionally copy it to GCS for backup or use elsewhere. See our [notebook](/tutorials/notebooks/SRADownload/) for an example.

Another important aspect of managing data storage costs is to be strategic about storing data in GCP vs. on your instances. When you have spun up a VM, you have already paid for the storage on the VM since you are paying for the size of the disk (storage space), whereas bucket storage is charged based on how much data you put in GCS. This is something to think about when copying results files back to GCS for example. If they are not files you will need later, then leave them on the VM's block storage and save your money on more important data to put into GCS. Just make sure you are always either backing up by creating a machine image (see below) or keeping data you can't live without in object-based cloud storage. And make sure you delete the instance when you are done with it, because storing old virtual machines can get expensive.

## **Spin up a Virtual Machine and run a workflow** <a name="vm"></a>
Google and other sources have a lot of great resources on how to spin up and use a VM. The first place we will point you is to the [NIH Common Data Fund resource](https://training.nih-cfde.org/en/latest/Cloud-Platforms/Introduction-to-GCP/gcp2/), which lays out how to spin up a VM, SSH into it, make a bucket, and move data around similar to what we did in the example notebooks above. One thing worth noting is that the NIH tutorial has you SSH into your instance using a gcloud command in the shell. This is one way to SSH in, but it is a lot easier to just double click the SSH button next to the instance name on the Compute Instances page. You can find the GCP specific documentation on how to spin up an instance [here](https://cloud.google.com/compute/docs/instances/create-start-instance#console). If you want to start a Windows VM, read [the documentation](https://cloud.google.com/compute/docs/quickstart-windows). Windows VMs can have extra costs associated with them, so make sure you understand what you are getting into before starting. We encourage you to follow our [auto-shutdown instructions](/docs/compute-engine-idle-shutdown.md) to prevent leaving machines running.

## **Disk Images** <a name="im"></a>
Part of the power of virtual machines is that they offer a blank slate for you to configure as desired. However, sometimes you want to recycle data or installed programs for your next VM instead of having to reinvent the wheel. One solution to this issue is using disk (or machine) images, where you copy your existing virtual disk to a [Machine Image](https://cloud.google.com/compute/docs/machine-images) which can serve as a backup or can be used to launch a new instance with the programs and data from a previous instance.

## **Launch a Jupyter Notebook** <a name="jup"></a>
Jupyter notebooks are web based interactive code platforms. On GCP, notebooks are launched through the Vertex AI platform. Here we are going to launch a JupyterLab environment on GCP, and then import a custom notebook from this repo to walk through running commands in Vertex AI. Vertex AI is Google's current approach to Machine Learning and Artificial Intelligence workflows. You can read more about a [Vertex AI Overview](https://cloud.google.com/vertex-ai) and [technical documentation and tutorials](https://cloud.google.com/vertex-ai/docs). 

To spin up a Notebook instance and import an example training notebook, follow our [guide here](/docs/vertexai.md).

## **Creating a Conda Environment** <a name="co"></a>
Virtual environments allow you to manage package versions without having package conflicts. For example, if you needed Python 3 for one analysis, but Python 2.7 for another, you could create separate environments to use the two versions of Python. One of the most popular package managers used for creating virtual environments is the [conda package manager](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html#:~:text=A%20conda%20environment%20is%20a,NumPy%201.6%20for%20legacy%20testing). 

Mamba is a reimplementation of conda written in C++ and runs much faster than legacy conda. Conda environments are created using configuration files in yaml format (yaml is a type of configuration file), or by listing the programs to install after the initial conda command. To create a conda environment on a virtual machine or Jupyter instance, follow [this guide](/docs/create_conda_env.md). We walk you through generating a generic conda environment on a Virtual Machine, as well as how to create a custom kernel for a notebook instance.

## **Managing Containers with Google Artifact Registry** <a name="dock"></a>
You can host containers within either the older Google Container Registry, or else in the newer Google Artifact Registry, which can host containers as well as other artifacts. We outline how to build a container, push to an Artifact Registry, and pull to a compute environment in our [docs](/docs/containers.md).

## **Serverless Functionality** <a name="ser"></a>

Serverless services are those that allow you to run things, an analysis, an app, a website etc., and not have to deal with servers (VMs). There are still servers running somewhere, you just don't have to manage them. All you have to do is call a command that runs your analysis in the background and copies the output files to a storage bucket. The most relevant serverless feature on GCP to Cloud Lab users (especially 'omics' analyses) is the [Google Cloud Life Sciences API](https://cloud.google.com/life-sciences/docs/reference/rest). You can walk through a tutorial of this service using this [notebook](/tutorials/notebooks/LifeSciencesAPI) Those doing health informatics should look into the [Google Cloud Healthcare Data Engine](https://cloud.google.com/healthcare). You can find a variety of other [tutorials](https://cloud.google.com/life-sciences/docs/tutorials) that leverage the Life Sciences API for life sciences applications, but we will point out that most of the examples are related to genomics. If you are doing other biomedical research related to imaging, NLP, or other fields, look at the [tutorials](/tutorials/) section of this repo for inspiration. Some of these also leverage the API from within the notebooks. Besides the Google-specific examples, you can use the Life Sciences API to run workflows using other workflow managers like [Snakemake](https://snakemake.readthedocs.io/en/stable/executing/cloud.html) or [Nextflow](https://cloud.google.com/life-sciences/docs/tutorials/nextflow). Google also just released a new service in preview called [Batch](https://cloud.google.com/blog/products/compute/new-batch-service-processes-batch-jobs-on-google-cloud) which is a compute scheduler that should feel similar to submitting jobs to a cluster. It will likely replace the Life Sciences API in the near future, but for now, you should experiment with both for submitting serverless jobs.

## **Clusters** <a name="clu"></a>
One great thing about the cloud is its ability to scale with demand. When you submit a job to a traditional computing cluster (a set of computers that work together to execute a function), you have to specify up front how many CPUs and how much memory you want to allocate to your job, and you may over- or under-utilize these resources. Alternatively, on the cloud you can leverage a feature called autoscaling, where the compute resources will scale up or down with the demand. This is more efficient and keeps costs down when demand is low, but prevents latency when demand is high (e.g. a whole hackathon submitting jobs to a cluster). For most users of Cloud Lab, the best way to benefit from autoscaling is to use an API like the Life Sciences API or Google BATCH, but in some cases, maybe for a whole lab group or a large project, it may make sense to spin up a [Kubernetes cluster](https://cloud.google.com/kubernetes-engine/docs/quickstart) and submit jobs to the cluster using a workflow manager like [Snakemake](https://snakemake.readthedocs.io/en/stable/executing/cloud.html). [One of our tutorials](/tutorials/notebooks/DL-gwas-gcp-example) uses a marketplace solution to deploy a small Kubernetes cluster and then run AI models using Kubeflow. Finally, you can spin up SLURM clusters on GCP, which is easiest to do through the Cloud Shell or a VM, but can be accessed via SSH from your terminal. Instructions are [here](https://cloud.google.com/architecture/deploying-slurm-cluster-compute-engine). 

## **Billing and Benchmarking** <a name="bb"></a>
Many Cloud Lab users are interested in understanding how to estimate the price of a large-scale project using a reduced sample size. Generally, you should be able to benchmark with a few representative samples to get an idea of the time and cost required for a larger scale project. In terms of cost, a great way to estimate costs is to use the [GCP cost calculator](https://cloud.google.com/products/calculator#id=b64e9c4f-c637-432f-8e2c-4a7238dde0f2) for an initial figure. The calculator is a tool that estimates costs based on location, VM type/size, and runtime. Then, you can run some benchmarks and double check that everything is acting as you expect. For example, if you know that your analysis on your on-premises cluster takes 4 hours to run for a single sample with 12 CPUs, and that each sample needs about 30 GB of storage to run a workflow, then you can extrapolate to determine total costs using the calculator (e.g., compute engine + cloud storage). 

To get a more precise estimate, you can use assign labels to your workflows, then generate a report for a specific label. You can learn how to do that in our [docs](docs/How%20to%20Create%20Labels%20and%20GCP%20Billing%20Report.docx.pdf). Note that it can take up to 24 hours to update the billing account, so you may need to wait a few hours after running an analysis before you will have an accurate report.

## **Cost Optimization** <a name="cost"></a>
As you go through all the tutorials, you can keep costs down by stopping and/or deleting resources (e.g., VMs or Buckets) that you no longer need. Another strategy is to ensure that you are using all the compute resources you have provisioned. If you spin up a VM with 16 CPUs, you can see if they are all being utilized using [Cloud Monitoring](https://cloud.google.com/monitoring#section-1). If you are only really using 8 CPUs for example, then just change your machine size to fit the analysis. You can also play with [Spot](https://cloud.google.com/spot-vms) instances for running workflows and end up saving a lot of money. Finally, you can create [Budget Alerts](https://cloud.google.com/billing/docs/how-to/budgets) to help you track your budget. 

## **Managing Your Code with the Google Source Repository** <a name="CODE"></a>
You if want an alternative to GitHub built directly into your cloud console, you can try the Google Cloud Source Repository. It uses all the normal git commands and will feel very familiar if you are used to using GitHub. If you want more background, or want to try it out, view our [guide](/docs/cloud_source_repository.md) in the docs.

## **Getting Support** <a name="sup"></a>
As part of the NIH Cloud Lab sign-up process, you will be added to the Cloud Lab Teams channel. Feel free to message others in the group for support and our team will also chime in and help. For NIH users, you can also submit a support ticket to the NIH IT Service Desk. For all other users, you can reach out to the Cloud Lab email with questions at `CloudLab@nih.gov`. Please be sure that your tickets/emails have a clear subject line, such as "GCP help with the Life Sciences API". For issues that the NIH Cloud Lab Support Team is unable to resolve, you can reach out to GCP enterprise support directly by clicking the question mark in the top right part of the console and opening a support case.

## **Additional Training** <a name="tr"></a>
This repo only scratches the surface of what can be done in the cloud. If you are interested in additional cloud training opportunities. Please visit the [STRIDES Training page](https://cloud.nih.gov/training/). For more information on the STRIDES Initiative at the NIH, visit [our website](https://cloud.nih.gov/) or contact the NIH STRIDES team at STRIDES@nih.gov for more information.
