---
title: "Module 5 - Scale up your app"
weight: 5
---

**Difficulty**: Beginner
**Estimated Time:** 10 minutes

The goal of this scenario is to scale a deployment with kubectl scale and to see the load balancing in action

## Step 1 - Scaling a deployment

First, let's recreate the Service we deleted in the previous module:

```shell
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

To list your deployments use the `get deployment` command:

```shell
kubectl get deployments
```

The output should be similar to:

```shell
NAME                  READY   UP-TO_DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           11m
```

We should have 1 Pod. If not, run the command again. This shows:

- *NAME* lists the names of the Deployments in the cluster.
- *READY* shows the ratio of CURRENT/DESIRED replicas
- *UP-TO-DATE* displays the number of replicas that have been updated to achieve the desired state.
- *AVAILABLE* displays how many replicas of the application are available to your users.
- *AGE* displays the amount of time that the application has been running.

To see the ReplicaSet created by the Deployment, run:

```shell
kubectl get rs
```

Notice that the name of the ReplicaSet is always formatted as `[DEPLOYMENT-NAME]-[RANDOM-STRING]`. The random string is randomly generated and uses the pod-template-hash as a seed.

Two important columns of this command are:

- *DESIRED* displays the desired number of replicas of the application, which you define when you create the Deployment. This is the desired state.
- *CURRENT* displays how many replicas are currently running.

Next, let's scale the Deployment to 4 replicas. We'll use the `kubectl scale` command, following by the deployment type, name and desired number of instances:

```shell
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

To list your Deployments once again, use `get deployments`:

```shell
kubectl get deployments
```

The change was applied, and we have 4 instances of the application available. Next, let's check if the number of Pods changed:
