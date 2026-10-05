# **Google Cloud Challenge Lab: ARC113 Reference Guide** ##

 **Cloud Pub/Sub and Cloud Scheduler Challenge Lab** This reference guide provides a verified, end-to-end walkthrough for **ARC113**. Each task reflects the exact commands executed step-by-step. 

Only the Region changes across student accounts. Verify your assigned region in the left-hand Lab Details panel before executing Step 1.
-----Step 1: Environment Setup & Variables
Run this block in Cloud Shell. Replace variables if your lab details panel lists a different region:
# 1. Assigned Region from the left panel (e.g., us-east4, us-central1)
export REGION="us-east4"

# 2. Topic name from Task 1 (e.g., gcloud-pubsub-topic or cloud-pubsub-topic)
export TOPIC_NAME="gcloud-pubsub-topic"

# 3. Subscription name from Task 1 (e.g., pubsub-subscription-message or cloud-pubsub-subscription)
export SUBSCRIPTION_NAME="pubsub-subscription-message"

# 4. Message string (e.g., "Hello World" or "Hello World!")
export MESSAGE_TEXT="Hello World"

# 5. Cloud Scheduler Job Name (used if Task 2 requires cron)
export SCHEDULER_JOB_NAME="cron-scheduler-job"

# Project environment config
export PROJECT_ID=$(gcloud config get-value project)
gcloud config set compute/region $REGION

echo "Configured for Project: PROJECTID|Region:REGION \vert{} Topic:$TOPIC_NAME"


-----Step 2: Task 1 — Set up Cloud Pub/Sub
Create the specified Cloud Pub/Sub topic and attach the subscription:
# 1. Create the Pub/Sub topic

gcloud pubsub topics create cloud-pubsub-topic

# 2. Create the subscription attached to the topic

gcloud pubsub subscriptions create cloud-pubsub-subscription \
    --topic=cloud-pubsub-topic

Checkpoint: Click Check my progress on Task 1: Set up Cloud Pub/Sub.
-----Step 3: Task 2 — Create a Cloud Scheduler Job
Configure a Cloud Scheduler cron job to publish the Hello World! payload to cloud-pubsub-topic every minute (* * * * *):
gcloud scheduler jobs create pubsub cron-scheduler-job \
  --location=$REGION \
  --schedule="* * * * *" \
  --topic=cloud-pubsub-topic \
  --message-body="Hello World!"

Checkpoint: Click Check my progress on Task 2: Create a Cloud Scheduler job.
-----Step 4: Task 3 — Verify the Results in Cloud Pub/Sub
Force-run the scheduler job so the message generates immediately, wait a few seconds for delivery, and pull the message from the subscription:
# 1. Manually trigger the job without waiting for the minute mark

gcloud scheduler jobs run cron-scheduler-job --location=$REGION

# 2. Allow 5 seconds for the message to propagate into the queue
sleep 5

# 3. Pull the published message from the subscription

gcloud pubsub subscriptions pull cloud-pubsub-subscription --limit 5

Checkpoint: Click Check my progress on Task 3: Verify the results in Cloud Pub/Sub. 


additional Notes:

🅰️ Variant A: Snapshot Workflow (Form 1)
Use this section if your lab lists:

Task 1: Publish a message to the topic

Task 2: View the message

Task 3: Create a Pub/Sub Snapshot for Pub/Sub topic

Bash
# ------------------------------------------------------------------------------
# Task 1: Create subscription & publish message to pre-created topic
# ------------------------------------------------------------------------------
# Note: Check if your task specifies 'pubsub-subscription-message'
gcloud pubsub subscriptions create pubsub-subscription-message \
    --topic=gcloud-pubsub-topic

gcloud pubsub topics publish gcloud-pubsub-topic \
    --message="Hello World"

# Checkpoint: Verify Task 1

# ------------------------------------------------------------------------------
# Task 2: View the message
# ------------------------------------------------------------------------------
gcloud pubsub subscriptions pull pubsub-subscription-message --limit 5

# Checkpoint: Verify Task 2

# ------------------------------------------------------------------------------
# Task 3: Create Pub/Sub Snapshot
# ------------------------------------------------------------------------------
gcloud pubsub snapshots create pubsub-snapshot \
    --subscription=gcloud-pubsub-subscription

# Checkpoint: Verify Task 3
🅱️ Variant B: Cloud Scheduler Workflow (Form 3)
Use this section if your lab lists:

Task 1: Set up Cloud Pub/Sub

Task 2: Create a Cloud Scheduler job

Task 3: Verify the results in Cloud Pub/Sub

Bash
# Set your Region from the left panel (e.g. us-central1, us-east4)
export REGION=""

# ------------------------------------------------------------------------------
# Task 1: Create topic and subscription
# ------------------------------------------------------------------------------
gcloud pubsub topics create cloud-pubsub-topic
gcloud pubsub subscriptions create cloud-pubsub-subscription \
    --topic=cloud-pubsub-topic

# Checkpoint: Verify Task 1

# ------------------------------------------------------------------------------
# Task 2: Create Cloud Scheduler Job
# ------------------------------------------------------------------------------
gcloud scheduler jobs create pubsub cron-scheduler-job \
    --location=$REGION \
    --schedule="* * * * *" \
    --topic=cloud-pubsub-topic \
    --message-body="Hello World!"

# Checkpoint: Verify Task 2

# ------------------------------------------------------------------------------
# Task 3: Run job & pull message
# ------------------------------------------------------------------------------
gcloud scheduler jobs run cron-scheduler-job --location=$REGION
sleep 5
gcloud pubsub subscriptions pull cloud-pubsub-subscription --limit 5

# Checkpoint: Verify Task 3

For a visual demonstration of the snapshot workflow steps, check out [Get Started with Pub/Sub Challenge Lab Video](https://www.youtube.com/watch?v=j7xz9e5mn8I).

This video walks through the exact commands and console verification for Form 1 of the ARC113 challenge lab.
http://googleusercontent.com/youtube_content/1
