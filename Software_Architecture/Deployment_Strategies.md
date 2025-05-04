## **Recreate Deployment Strategy**

We stop and then recreate the application. The newly created deployment runs the updated version of the application.

- Users will experience some downtime because we need to stop the application before the new version is running.
- This strategy is easy to set up.
- Rollback causes downtime

## **Blue-Green Deployment**

The original, old version is called blue environment, and the new updated version is called green environment. Both environments are running our application, but the users are still using the old version. The next step is to switch to a green environment by transferring all user traffic to it. This switch can happen very quickly, so users won’t experience downtime.

When the deployment is finished and we don’t want to roll back, we can remove the old, blue environment. Then we rename the updated version from green to blue.

- No downtime
- Rollback happens immideatlely
- Higher resource needs
- Might run into problems with the rollback if the new version makes some non-backward-compatible changes.
- Difficult to setup

## **Rolling Update**

It is applicable only if we have multiple instances of an application. Firstly, we create a new instance that runs the updated version. When this new instance is started, we consider it a part of the application, so the user requests might already be processed by it:

[![](https://lh3.googleusercontent.com/7hHBh36lTKv2Fr06DPdr4KNdOugApjdJ6ckj5JwUnu1l6XT9_pGuJsMVH1qTPWBuIqRS2Jf7Jm1xJPfXfHrLjTPrE-dj7IzcfWyXF46B7KDrKv9fppImDTqd7NQuFG25-xnBEhOQK9Y6vJzSnQDhy9f35I5sx4zQ09SVMEpHbV0LUqFlOnxOp-vwKbPe)](https://lh3.googleusercontent.com/7hHBh36lTKv2Fr06DPdr4KNdOugApjdJ6ckj5JwUnu1l6XT9_pGuJsMVH1qTPWBuIqRS2Jf7Jm1xJPfXfHrLjTPrE-dj7IzcfWyXF46B7KDrKv9fppImDTqd7NQuFG25-xnBEhOQK9Y6vJzSnQDhy9f35I5sx4zQ09SVMEpHbV0LUqFlOnxOp-vwKbPe)

After this, we remove one from the instances that run the old version. The process continues to add instances running the new version and remove their pairs. The deployment is finished when the last instance is replaced with an updated one, and we made sure that the application works as expected.

## **Canary Deployment**

The main idea is that we deploy the update to more and more users in iterations. Initially, only a small part of the users receive the update. When we are sure that the update is successful, we can continue the rollout to all of the users.  For example 25%, 50%, 75%, and then 100%

- provide a high level of control, but this might be difficult to implement
- We need to select the target audience who gets the updates first. For ex depending on device type

## **Shadow Deployment**

Shadow deployment is similar to blue-green deployment in the sense that it uses two identical environments. When we use shadow deployment, both environments receive the requests, but the responses come from the original application version

- we don’t have the risk of introducing bugs to the system while we can monitor and test the new version under load
- we need to ensure that the new version doesn’t have any side effects.