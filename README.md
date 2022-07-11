# inc-tac-fedtest

The Federation Test Environment Working Group is chartered by the InCommon Technical Advisory Committee to identify and prioritize relevant testing and validation best practices, procedures, tooling, and environments when:

* An existing or prospective InCommon Participant integrates services (identity provider and service provider) via the InCommon Federation 
* The InCommon federation operator introduces changes to federation infrastructure

Please see the [Working Group](https://spaces.at.internet2.edu/display/fedtestwg) pages.


Next meeting:

1. Consider whether the [demo specification](demo-specifications/SDP-EX01/test-outline.md) is an example of what is needed, erring on the side of too comprehensive.
2. Consider whether the [demo specification](demo-specifications/SDP-EX01/test-outline.md) has most of the elements needed for a template
3. Address the "TODO"s below.

Once we have a satisfactory template, either

1. create a directory for each of the deployment statements, populate with a (customized?) template for each
2. invite community to develop tests using the template in the directory and submit for review
3. iterate?


## Premise of test specification design

### The test design MUST take various metadata configurations into account

Given a test subject (IdP or SP), representative of the test subject will be expected to integrate with the federation test metadata via MDQ in one of three ways.

* simply pointing at MDQ end point, most mature,
* receiving a list of entities to be added (maximal and minimal list),
* receiving the MDQ URLs to be added (maximal and minimal list).

The first configuration can have additional counterpart entities added during the test; the remaining configurations need the test entities known in advance.

TODO discuss with team and determine if this is reasonable list; change the template tables to reflect max/min config lists for bilateral.

### The test design MUST specify the values needed from the test entity to test the deployment statement

The representative of the test subject would fill out a configuration form (or submit a configuration file).  (Leaving it up to the implementation teams to decide how to reuse elements.)

### The test design SHOULD list the possible outcomes of the test and assign a score to each

TODO discuss scoring options: PASS/FAIL; BEST/BETTER/GOOD/ACCEPTABLE/BUGGY/FAIL 

### The test design COULD provide a weighting of all the tests

In order to understand the relative importance of the results of various tests, the test design can specify the weighting to be accorded to each score. For example, a test could determine whether the test subject handles a correctly configured counterpart and weight that test highly. A second test could determine whether the test subject reasonably handles a plausibly misconfigured or extreme configured counterpart, and that test could be weighed far less.

For extreme configuration, consider a metadata element that is multivalued. An extreme configuration could have ten times the number of values a common configuration has.
