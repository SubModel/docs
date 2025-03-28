# Manage Pods

Learn how to start, stop, and manage Pods using SubModel. This guide covers creating and terminating Pods.

If you're unsure which Pod best suits your needs, refer to [Choose a Pod](/pods/choose-a-pod.md).

## Create Pods

1. Navigate to [Pods](https://submodel.ai/#/inst/list) and click **+ Deploy**.
2. Choose between **GPU** and **CPU**.
3. Customize your instance by configuring the following:
   1. (Optional) Specify a Network volume.
   2. Select an instance type, such as **A40**.
   3. (Optional) Provide a template, for example, **SubModel Pytorch**.
   4. (GPU only) Specify your compute count.
4. Review your configuration and click **Deploy On-Demand**.


## Stop a Pod

1. Click the stop icon next to the Pod you wish to stop.
2. Confirm by clicking the **Stop Pod** button.

### Stop a Pod After a Specific Time

You can also stop a Pod after a specific amount of time. For example, the following command will stop the Pod after 2 hours.

#### SSH

Use the following command to stop a Pod after 2 hours:

```bash
sleep 2h; submodelctl stop pod $SUBMODEL_POD_ID &
```

This command uses `sleep` to wait for 2 hours before executing the `submodelctl stop pod` command to stop the Pod. The `&` at the end runs the command in the background, allowing you to continue using the SSH session.

#### Web Terminal

To stop a Pod after 2 hours using the web terminal, enter:

```bash
nohup bash -c "sleep 2h; submodelctl stop pod $SUBMODEL_POD_ID" &
```

The `nohup` command ensures the process continues running even if you close the web terminal window.

> **Warning**
>
> You are charged for storing idle Pods. If you do not need to store your Pod, make sure to terminate it.

## Start a Pod

You can resume a Pod that has been stopped.

1. Navigate to the **Pods** page.
2. Select the Pod you want to resume.
3. Click **Start**.

Your Pod will resume operation.


## Terminate a Pod

> **Danger**
>
> Terminating a Pod permanently deletes all data outside your [Network volume](/pods/storage/create-network-volumes). Ensure you've saved any data you want to access again.


1. Click the hamburger menu at the bottom of the Pod you want to terminate.
2. Select **Terminate Pod**.
3. Confirm by clicking the **Yes** button.
