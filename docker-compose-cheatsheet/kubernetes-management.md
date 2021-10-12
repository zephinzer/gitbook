# ☸️ Kubernetes Management

## Delete a Namespace stuck in `Terminating` state

{% hint style="info" %}
Note that using `kubectl edit namespace $NAMESPACE` **WILL NOT WORK. **It has to go through `kubectl replace ...`
{% endhint %}

1. Identify the namespace that is stuck using `kubectl get namespaces` (this shall be `$NAMESPACE`)
2. Run the following to get the Namespace resource as JSON:\
   `kubectl get namespace $NAMESPACE -o json > $NAMESPACE.json`
3. Modify `$NAMESPACE.json` and set the property at `.spec.finalizers` to `[]`
4. Run the following to apply the Namespace resource:\
   `kubectl replace --raw "/api/v1/namespaces/$NAMESPACE/finalize -f ./$NAMESPACE.json`
5. Your Namespace resource should be deleted, check with `kubectl get namespaces`
