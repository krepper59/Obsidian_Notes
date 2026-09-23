### SIEM Data Flow
```
1. Ingests logs from various sources 
2. Data logs are processed and normalized to be understood by the SIEM correlation engine (normalization and aggregation)
3. Data is utilized to create detection rules, dashboards, visualizations, alerts, and incidents
```
### Elastic Stack - Elastic, Logstash, & Kibana
```
Elasticsearch - JSON-based search engine with RESTful API's. Handles indexing, storing, and querying of data.
Logstash - collects, transforms, and transports log files
	1. Process Input: ingests log files and converts them to machine readable logs
	2. Transform & Enrich: modification of log records and data to fit the needs of the SIEM
	3. Send Logs to Elasticsearch: processed logs are transmitted to Elasticsearch for querying
Kibana - visualization tool for Elasticsearch documents where queries are executed using Elasticsearch
Beats - lightweight data shippers installed on remote machines to ship logs to logstash and elasticsearch
```
###### KQL - Kibana Query Language
```
Basic Structure: 
	field:value (Ex: event.code:4625)
Free Text Search:
	"<search terms>" (Ex: "svc-sql1")
Logical Operators:
	AND|OR|NOT (Ex: event.code:4625 AND winlog.event_data.SubStatus:0xC0000072)
Comparison Operators:
	:, :>, :>=, :<, :<=, and :!
Wildcards & Regular Expressions:
	Only used in value fields (Ex: event.code:4625 AND user.name: admin*)

Elastic Documentation:
https://www.elastic.co/guide/en/ecs/current/ecs-reference.html                               ECS
https://www.elastic.co/guide/en/ecs/current/ecs-event.html                                   Event Fields
https://www.elastic.co/guide/en/beats/winlogbeat/current/exported-fields-winlog.html         Winlogbeat Fields
https://www.elastic.co/guide/en/beats/winlogbeat/current/exported-fields-ecs.html            Winlogbeat ECS Fields
https://www.elastic.co/guide/en/beats/winlogbeat/current/exported-fields-security.html       Winlogbeat Security Module Fields
https://www.elastic.co/guide/en/beats/filebeat/current/exported-fields.html                  Filebeat Fields
https://www.elastic.co/guide/en/beats/filebeat/current/exported-fields-ecs.html              Filebeat ECS Fields
```
###### Elastic Common Schema (ECS)
```
Vocabulary for events and logs across the Elastic Stack.
Advantages of KQL in ECS:
	1. Unified Data View - data from multiple sources can be searched using the same fields
	2. Improved Search Efficiency - standardized names simplify KQL queries
	3. Enhanced Correlation - data from different sources can be correlated together
	4. Better Visualizations - consistent field names simplifies visualization efforts in Kibana
	5. Interoperability with Elastic Solutions - Elastic Security, Elastic Observability, Elastic Machine Learning
	6. Future-proofing - new features and enhancements seamlessly integrate
```
#### Diagrams
![[Pasted image 20260922192913.png|635]]
![[Pasted image 20260922192925.png|638]]
