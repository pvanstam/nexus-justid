..  NEXUSe2e for Justid - Overview
	Gives an overview of the installation and config of Nexuse2e

Overview
========

Providers of telecommunication providers need to communicate with CIOT for sending files to the Justice Department. The communication channel between partners is standardized by ebms/ebXML. Communication is with the Justid organization.

NEXUSe2e is a messaging application that is capable of messaging with the ebXML protocol.

In this documentation the efforts to use NEXUSe2e for communicating with Justid is documented. This documentation is provided as-is. No claims can be made by using this documentation. This documentation is not official documentation of the Dutch Government, but just work from volunteers. A lot of providers used this document or previous versions, therefore it can be marked as a proven configuration.


Getting the CPA
---------------

To be able to transfer the CIOT files the following actions must be taken.

1. Register at Justid for the CIOT connection. You will receive the application form form and account. After submitting the application you will receive an account for the `CPA Register <https://cparegister.minvenj.nl/dashboard/>`__.
2. `Create a connection <https://cparegister.minvenj.nl/process/participantDetails/create/>`__.
3. Create and `download the CPA <https://cparegister.minvenj.nl/cpa/overzicht/>`__.
4. Install and configure NEXUSe2e

The CPA contains the necessary information for configuring NEXUSe2e.
