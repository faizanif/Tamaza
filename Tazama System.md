||||
| :- | :-: | -: |

**Table of Contents**


[1.	Purpose:	1](#_toc859007637)

[2.	Features:	1](#_toc1771111021)

[3.	How Tazama works:	2](#_toc633242218)

[What is a Semi- Attached System?	2](#_toc143834336)

[Detection Ecosystem:	2](#_toc1308093454)

[Tazama API Architecture	3](#_toc657385436)

[What is ISO 20022 Message Formats Supported by the TMS API?	4](#_toc1842016413)

[I.	Payment Initiation Messages	4](#_toc73808397)

[II.	Payment Settlement Messages	5](#_toc827203479)

[III.	How TMS API Uses These Messages	5](#_toc1865139032)

[1.	Data Ingestion:	5](#_toc1274105033)

[2.	Rule Processing:	5](#_toc1789949946)

[3.	Alerting and Reporting:	5](#_toc1346963624)

[4.	Data Preparation:	6](#_toc1454749329)

[i.	Store the message as received	7](#_toc1984727612)

[ii.	Load the message into the historical graph	7](#_toc838390011)

[iii.	Create the DataCache object	7](#_toc423819979)

[5.	Message Transmission	8](#_toc2024148900)

[6.	Payment Platform Adapters	8](#_toc984339498)









1. # <a name="_toc859007637"></a>Purpose:
The purpose of Tazama System is to monitor and evaluate the transactions with the help of Transaction Monitoring Service (TMS) API to avoid any fraud, financial crime and money laundering. 
1. # <a name="_toc1771111021"></a>Features:
- Real-Time Data Ingestion: It captures real-time data through TMS - Transaction Monitoring Service (TMS) API
- Event Routing: the transactions are directed by the Event Director to appropriate rule processors *in tazama, it ensures that each transaction is evaluated to avoid fraudulent or suspicious behavior.* 
- Rule Evaluation: Transaction went through to pre-defined criteria called as *Rule configure in tazama.* each designed to assess specific aspects of a transaction i.e. tenure of an account, Relationship between sender and receiver.
- Typology Scoring: Results from the rule evaluations are aggregated by the Typology Processor, to determine if the transaction exhibits characteristics of known fraud or money laundering scenarios (typologies)
- Alert Generation and Action: When suspicious activity is detected, Tazama can issue alerts to external case management systems for further investigation.
1. # <a name="_toc633242218"></a>How Tazama works:
When a transaction is made by customer it goes through a process of transaction messages from customer systems, evaluate these messages for specific behaviors i.e. rules configure in system and deliver the outcome of an assessment. Tazama system deploy as a service, it needs a:

1. single Financial Service Provider (FSP), 
1. An intermediary system such as a clearing house or payment switch
1. A combination of FSP and switching participants

The above process called a "semi-attached" configuration
## `   `<a name="_toc143834336"></a>What is a Semi- Attached System?
Tazama operates as a standalone or external system that connects via APIs. It has independent processing. It relies on real-time data ingestion from FSP but processes this data independently to detect fraud or suspicious activities. It can send outcome or feedback to provider’s system.
### <a name="_toc1308093454"></a>Detection Ecosystem:
1) Transction is centralized monitoring via switch operator
1) Open interface it received transaction form switch participants 
1) Switch operators evaluate all transactions via Tazama
1) Each participants have its own comply teams
1) Some FSPs has its own detection capabilities

   ![](Aspose.Words.06494f9b-d353-4b5c-9acf-9e10876204d7.001.png)

The switching hub and FSPs both inside and outside the switching ecosystem can submit transaction messages to the Tazama system for evaluation.

## <a name="_toc657385436"></a>Tazama API Architecture

![](Aspose.Words.06494f9b-d353-4b5c-9acf-9e10876204d7.002.png)

The TMS API implements ISO 20022 message formats to facilitate Payment Initiation messages pain.001 and pain.013 and Payment Settlement messages pacs.008 and pacs.002. By default, Tazama is set up to evaluate four transactions composed into a two-stage quote-and-transfer process:

Tazama has the ability to evaluate the transaction before completion and to allow a transaction to be blocked.
### <a name="_toc1842016413"></a>**What is ISO 20022 Message Formats Supported by the TMS API?**
Here’s an overview of the supported message types and their purposes:
1. #### <a name="_toc73808397"></a>***Payment Initiation Messages***
- pain.001: Customer Credit Transfer Initiation
  - Purpose: Used to initiate credit transfer payments. It contains information about the payment, including:
    - Debtor (payer) details.
    - Creditor (payee) details.
    - Amount, currency, and payment instructions.
  - Role in Tazama: Provides transaction details for fraud or compliance checks.
- pain.013: Creditor Payment Activation Request
  - Purpose: Requests payment activation from the creditor's side in cases where creditors need to initiate transactions.
  - Role in Tazama: Helps detect fraud patterns originating from the creditor side, such as account manipulation or unauthorized activations.

1. #### <a name="_toc827203479"></a>***Payment Settlement Messages***
- pacs.008: Financial Institution Credit Transfer
  - Purpose: Used for interbank credit transfer settlement between financial institutions.
  - Role in Tazama: Monitors high-value transactions for compliance and fraud detection in settlement flows.
- pacs.002: Payment Status Report
  - Purpose: Provides the status of a previously initiated payment, such as successful, pending, or rejected.
  - Role in Tazama: Allows the system to monitor the outcomes of flagged or suspicious transactions.

1. #### <a name="_toc1865139032"></a>*How TMS API Uses These Messages*
1. ##### <a name="_toc1274105033"></a>*Data Ingestion:*
   1. The TMS API captures transaction data from these ISO 20022 messages in real time.
   1. For example:
      1. pain.001 provides the details of an initiated credit transfer.
      1. pacs.008 ensures settlement between financial institutions is monitored.
1. ##### <a name="_toc1789949946"></a>*Rule Processing:*
   1. Data extracted from these messages is evaluated against fraud detection rules and typologies.
   1. Example: A pain.001 message for a high-value transfer might trigger rules related to unusually large payments.
1. ##### <a name="_toc1346963624"></a>*Alerting and Reporting:*
   1. Based on the evaluation, Tazama generates alerts for suspicious transactions.
   1. Information from pacs.002 can be used to confirm whether a flagged transaction was successfully completed or rejected.

![](Aspose.Words.06494f9b-d353-4b5c-9acf-9e10876204d7.003.png)

With Tazama and the TMS API up and running, you can send the messages to their respective endpoints

The TMS API adheres to the OpenAPI specification, utilizing a Swagger document to validate incoming messages. This ensures that each message complies with the ISO 20022 standard and includes all necessary information required for successful evaluation

1. # <a name="_toc1454749329"></a>Data Preparation:
When a transaction is received successfully and validated by the TMS API then the system does*** initial processing of the transaction message. This step involves integrating the message into the Tazama historical data store, enabling the system to store and analyze the transaction data for future reference and monitoring.

![](Aspose.Words.06494f9b-d353-4b5c-9acf-9e10876204d7.004.png)

When each message is received and processed by the TMS API, the following tasks are performed:
1. #### <a name="_toc1984727612"></a>*Store the message as received*
For audit purposes, incoming messages are stored unadulterated in separate data collections.
1. #### <a name="_toc838390011"></a>*Load the message into the historical graph*
The bulk of the behavioral modelling is performed over data composed into the historical graph database.
1. #### <a name="_toc423819979"></a>*Create the DataCache object*
1. The chain of four messages that comprises a complete transaction from quote to transfer contains the same information for some basic components that are used to interact with the graph database. For better performance, rule processors can retrieve the necessary basic information from the DataCache within the payloads.

1. *Generate meta-data*

   Two additional information added in transaction message;

1. Firstly, we create a traceParent attribute that will generate an over-arching identifier that we use to link logging across all the processors together. 
1. Secondly, we record the time it took to process the transaction inside the TMS API and Data Preparation into a prcgTmDP attribute.

5. # <a name="_toc2024148900"></a>Message Transmission
   On the conclusion of the Data Preparation process, the complete message payload is sent from the Transaction Monitoring Service API to the Event Director.
5. # <a name="_toc984339498"></a>Payment Platform Adapters
   If tazama is not able to submit messages in a require format of ISO2022 format, client need to submit the transactions as per the requirement and then passed to the Tazama system to meet the specification of the Tazama Transaction Monitoring Service (TMS) API.
||||
| :- | :-: | -: |

