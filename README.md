Deriving THT Parameters Using Mistral - POC
===========================================

This is a PoC that demonstrates using a Mistral workflow to derive parameters
that can be fed back into a TripleO overcloud deployment. For background, see
the [Deriving TripleO Parameters](https://review.openstack.org/#/c/423304)
spec. Also see the [Derived THT
POC](https://github.com/fultonj/derived-tht-poc), which is an earlier effort
that uses python instead of Mistral.

This PoC demonstates how a HCI performance profile can be used to derive two
of critical THT parameters for Nova. The PoC consists of two files:
- hci_parameters.yaml defines a Mistral workflow that derives the Nova
parameters.
- hci_inputs.json is a JSON file that contains the inputs to the Mistral
workflow.

The PoC can be executed on any system that has Mistral installed, such as the
TripleO undercloud.
1. Create the Mistal workflow
>$ mistral workflow-create hci_parameters.yaml
2. Execute the workflow
>$ mistral execution-create derive_hci_params hci_inputs.json
3. Examine the output (use the ID value from the previous command)
>$ mistral execution-get-output ID 
4. Clean up (delete the execution)
>$ mistral execution-delete ID 

The Mistral output should report a number of output values, and the important
ones are:
- nova::compute::reserved_host_memory
- nova::cpu_allocation_ratio

These are the THT environment variables that need to be fed back into the
overcloud deployment plan.
