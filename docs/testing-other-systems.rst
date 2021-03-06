.. _testing-other-systems:

===========================================
Testing other systems using custom clients
===========================================

Locust was built with HTTP as its main target. However, it can easily be extended to load test 
any request/response based system, by writing a custom client that triggers 
:py:attr:`request_success <locust.event.Events.request_success>` and 
:py:attr:`request_failure <locust.event.Events.request_failure>` events.

Sample XML-RPC User client
============================

Here is an example of a User class, **XmlRpcUser**, which provides an XML-RPC client,
**XmlRpcUser**, and tracks all requests made:

.. literalinclude:: ../examples/custom_xmlrpc_client/xmlrpc_locustfile.py

If you've written Locust tests before, you'll recognize the class called ``ApiUser`` which is a normal 
User class that has a couple of tasks declared. However, the ``ApiUser`` inherits from
``XmlRpcUser`` that you can see right above ``ApiUser``. The ``XmlRpcUser`` is marked as abstract
using ``abstract = True`` which means that Locust till not try to create simulated users from that class 
(only of classes that extends it). ``XmlRpcUser`` provides an instance of XmlRpcClient under the
``client`` attribute. 

The ``XmlRpcClient`` is a wrapper around the standard 
library's :py:class:`xmlrpc.client.ServerProxy`. It  basically just proxies the function calls, but with the 
important addition of firing :py:attr:`locust.event.Events.request_success` and :py:attr:`locust.event.Events.request_failure` 
events, which will record all calls in Locust's statistics.

Here's an implementation of an XML-RPC server that would work as a server for the code above:

.. literalinclude:: ../examples/custom_xmlrpc_client/server.py

For more examples, see `locust-plugins <https://github.com/SvenskaSpel/locust-plugins#users>`_
