# Future Considerations

## Performance

This web app is not yet optimized for "cold starts," and the
[WEBrick](https://docs.ruby-lang.org/en/2.4.0/WEBrick.html) HTTP server should
be replaced with a more performant web server. However, Cloud Run will scale the
service automatically, increasing the number of replicas horizontally as demand
increases.

Though the Firestore key is indexed, and Firestore in "datastore mode" can scale
to millions of operations per second, a real web application would likely
benefit from a caching tier that sits between it and the database. This may also
reduce cost at scale. See the [Designing for
Scale](https://cloud.google.com/datastore/docs/best-practices#designing_for_scale)
section of Google's "Datastore Best Practices" article for more information
about key distribution, contention (hot spots), latency, and ramp-up.

## Rollout

Real-world production applications use various upgrade strategies when deploying
new versions. For example, Cloud Run allows you to specify how much traffic a
new revision should receive. You may specify that 5% of requests go to the new
revision for a period of time, and if error rates and other conditions remain
stable, you could increase the percentage until you're at 100%. If errors are
encountered, a rollback can be performed.

## Security

A WAF similar to [Cloud Armor](https://cloud.google.com/armor) would be used
in production to help identify and mitigate DDoS attacks, etc.

### GCP Authentication

Authentication logic for Firestore is not handled by this application because
the default IAM policies used by the Cloud Run Service Agent allow various
traffic and permissions within the same GCP Project.

### IAM Policies

In a real application environment, more granular IAM policies would be crafted
and bound to dedicated Service Accounts, yielding a "least privilege" model for
services which provision and consume other services and resources.

## Pricing

There was not time to conduct forecasting or major comparison for various
architectures as they relate to operational expenditure (OpEx), though the
author is highly aware of these considerations at scale.

For example, when does [GKE](https://cloud.google.com/kubernetes-engine) become
cheaper than Cloud Run (or vice versa)? What about storage systems? Egress
transit? Sustained-use and/or committment discounts? etc. Many organizations
form Center Of Excellence (COE) teams who become experts in optimizing cloud
spend.
