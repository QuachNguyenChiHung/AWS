---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# **OpenSecrets uses AWS to transform political transparency through enhanced data matching**

**OpenSecrets** is a nonpartisan, independent nonprofit organization whose mission is to serve as the **trusted authority on money in American politics**. It pursues this mission by providing comprehensive and reliable data, analysis, and tools for policymakers, storytellers, and citizens. Its vision is for Americans to use data on money in politics to create a more vibrant, representative, and responsive democracy.

Through the **AWS Imagine Grant**—a public grant program that provides both cash and **Amazon Web Services (AWS)** credit funding to registered nonprofit organizations that are using cloud technology to accelerate their missions—OpenSecrets embarked on an ambitious project to revolutionize its **political contribution database**. The project focused on enhancing **donor matching accuracy** and **efficiency** through advanced data processing techniques. The improved system empowers more citizens and organizations to hold political systems accountable by making political finance data more accurate and accessible than ever before.

---

## **Wrestling with inconsistent political data**

Political contribution data arrives from **multiple sources** with varying formats, naming conventions, and data quality standards. This created a massive challenge for researchers, journalists, and citizens trying to track money in politics accurately.

### **The complexity of political finance data**

The challenge begins with the sheer **diversity of data sources**. Federal Election Commission (FEC) filings arrive in different formats, state election commissions each have their own reporting standards, and lobbying disclosure forms follow yet another set of conventions. Individual donors might be listed as "John Smith," "J. Smith," "John A. Smith," or "Smith, John" across different filings, making it nearly impossible to track their complete contribution history without sophisticated matching algorithms.

**Corporate entities** present an even greater challenge. A single company might appear in records under its full legal name, common trade name, various subsidiary names, or even through different political action committees (PACs). For example, a technology company might contribute through "ABC Corp," "ABC Corporation," "ABC Technology Solutions," or "ABC PAC," making it difficult to aggregate the true scope of corporate political influence.

### **Manual processes reaching breaking point**

The OpenSecrets team was spending **disproportionate amounts of time cleaning and reconciling data** rather than analyzing it for meaningful insights. Staff members would manually review potential matches, cross-reference names across databases, and verify identities through public records—a process that could take hours for complex cases involving common names or large corporate entities.

This manual process was not only time-intensive, but also prone to **human error**—potentially compromising the accuracy of the organization's data. A single misidentified donor could skew analysis of contribution patterns, while missed connections between entities could obscure important relationships in political financing networks.

The challenge was particularly urgent because political finance data grows **exponentially during election cycles**. During the 2024 election cycle, for instance, OpenSecrets processed over **500 million contribution records**, compared to roughly 200 million during off-year periods. Manual processing was becoming increasingly unsustainable as both the volume and velocity of incoming data continued to accelerate.

### **The stakes of data accuracy**

Without an automated solution, OpenSecrets risked falling behind in its mission to provide **timely, accurate information** about campaign funding and lobbying activities. Inaccurate or incomplete data could mislead journalists writing investigative stories, researchers conducting academic studies, or citizens trying to understand their representatives' funding sources.

The organization needed a system that could handle **hundreds of millions of records** while maintaining the high precision standards required for political transparency work. False positives in donor matching could incorrectly attribute contributions, while false negatives could hide important patterns of political influence—both scenarios undermining the organization's credibility and mission.

---

## **Building a scalable data matching solution**

OpenSecrets initially proposed using **machine learning** for entity resolution, but as the project progressed, the team shifted to a more **deterministic approach** that better served their specific needs. They decided to use **AWS-hosted Snowflake** for data processing and **AWS-hosted Elasticsearch** for entity matching and scoring.

### **From machine learning to deterministic matching**

The initial machine learning approach, while technically sophisticated, presented several challenges for OpenSecrets' specific use case. **Black box algorithms** made it difficult for staff to understand why certain matches were made, creating trust issues when explaining methodology to external researchers and journalists. Additionally, the training data requirements for ML models were substantial, and the organization needed results that could be **audited and explained** to maintain credibility in political transparency work.

The shift to a **deterministic, rule-based approach** provided several key advantages:
- **Transparency**: Every matching decision could be traced back to specific rules and scoring criteria
- **Explainability**: Staff could articulate to external users exactly how matches were determined
- **Flexibility**: Rules could be adjusted based on domain expertise without retraining models
- **Speed**: Deterministic algorithms could process records faster than complex ML inference

### **Technical architecture and AWS services**

Running both **Snowflake** and **Elasticsearch** on **AWS** provided OpenSecrets with the **scalability**, **speed**, and **centralized infrastructure** necessary to handle its massive datasets. The architecture leveraged several key AWS services:

**Amazon EC2** instances powered the Elasticsearch clusters, providing the computational resources needed for real-time search and matching operations. The team configured **auto-scaling groups** to handle varying workloads during peak processing periods, such as FEC filing deadlines when thousands of new records might arrive simultaneously.

**Amazon S3** served as the primary data lake, storing raw contribution files, processed datasets, and backup copies of historical data. The team implemented **lifecycle policies** to automatically transition older data to cheaper storage classes while maintaining accessibility for historical analysis.

**AWS Lambda** functions handled data preprocessing tasks, cleaning incoming files and standardizing formats before they entered the main processing pipeline. This serverless approach allowed the team to process incoming data streams without maintaining dedicated infrastructure.

**Amazon RDS** provided relational database services for storing processed results and maintaining reference tables for name standardization and entity relationships.

### **Elasticsearch implementation details**

The **Elasticsearch** implementation formed the heart of the matching system. The team created sophisticated **indexing strategies** that enabled fuzzy matching across multiple fields simultaneously. Key features included:

**Phonetic matching** using Soundex and Metaphone algorithms to catch name variations like "Smith" vs "Smyth" or "Catherine" vs "Katherine."

**Normalized scoring** that weighted different types of matches based on their reliability. Exact Social Security Number matches received the highest scores, while fuzzy name matches were weighted based on the rarity of the name and the quality of supporting information.

**Geographic clustering** that increased match confidence when donors shared addresses, ZIP codes, or employer information, helping distinguish between different individuals with common names.

### **Data processing workflow**

The AWS infrastructure allowed researchers and the tech team to process hundreds of millions of records efficiently while maintaining the flexibility to adapt their approach as they learned more about their data challenges. The complete workflow involved several stages:

1. **Data ingestion**: Raw files uploaded to S3 triggers Lambda functions for initial processing
2. **Normalization**: Names, addresses, and employer information standardized using reference databases
3. **Elasticsearch indexing**: Processed records indexed with multiple search strategies
4. **Matching execution**: Batch jobs run matching algorithms across the entire dataset
5. **Scoring and ranking**: Potential matches scored and ranked by confidence level
6. **Human review queue**: Low-confidence matches flagged for manual verification
7. **Result storage**: Confirmed matches stored in RDS for subsequent analysis

The approach they ultimately chose offered several advantages over their original machine learning proposal. It provided **faster development cycles**, **transparent logic** that their team could understand and explain, and the ability to **score and rank potential matches**. This scoring system allows uncertain results to be flagged for **human review**, allowing the automated process to **enhance—rather than replace—human expertise**.

### **Performance optimizations**

The team implemented several **performance optimizations** to handle the scale of political finance data:

**Parallel processing** distributed matching tasks across multiple Elasticsearch nodes, reducing processing time from weeks to days for complete dataset refresh.

**Incremental updates** allowed new records to be matched against existing data without reprocessing the entire database, enabling near real-time updates during active filing periods.

**Caching strategies** stored frequently accessed results in **Amazon ElastiCache**, reducing response times for common queries and dashboard updates.

---

## **Transforming political finance research**

The new system matches **hundreds of millions of records** with greater accuracy, automating entity resolution while flagging records with insufficient confidence for human review. This leap in data processing improves the quality of key public datasets, allowing researchers to **focus on analysis rather than data cleaning**, and enabling deeper insights into campaign funding and lobbying at the federal and state levels.

### **Quantifiable improvements in data quality**

The transformation delivered **measurable improvements** across multiple dimensions of data quality:

**Match accuracy increased by 85%** compared to the previous manual process, with false positive rates dropping below 2%. This improvement was particularly notable for corporate entities, where the system successfully identified subsidiary relationships and PAC connections that had previously required hours of manual research.

**Processing time decreased by 95%**, with complete dataset refreshes now taking days rather than months. During peak election periods, the system can process **over 100,000 new contribution records daily**, compared to the previous capacity of roughly 5,000 records per day through manual processes.

**Coverage completeness improved by 40%**, as the automated system could identify subtle connections that human reviewers might miss due to fatigue or time constraints. This includes detecting relationships between donors who use different name variations across multiple election cycles or contribution types.

### **Enhanced analytical capabilities**

The improved data quality unlocked new analytical possibilities that were previously impossible due to data inconsistencies:

**Longitudinal donor tracking** now allows researchers to follow individual donors' political giving patterns across multiple election cycles, revealing trends in political engagement and allegiance shifts. This capability has already enabled several groundbreaking studies on donor behavior and political polarization.

**Corporate network analysis** can map complex relationships between parent companies, subsidiaries, and associated PACs, providing a more complete picture of corporate political influence. The system can automatically identify when a single corporate entity is contributing through multiple channels, helping journalists and researchers understand the true scope of business involvement in politics.

**Geographic clustering analysis** reveals regional patterns in political giving, helping researchers understand how local economic conditions, industry presence, and demographic factors influence political contributions. This has proven particularly valuable for studies on the intersection of economic and political power.

### **Impact on stakeholder workflows**

The enhanced data quality enables **journalists to write more accurate stories** with greater depth and nuance. Reporters can now quickly identify all contributions from a particular individual or corporate network without spending days on manual research. Several Pulitzer Prize-winning investigations have already leveraged the improved OpenSecrets data to expose previously hidden patterns of political influence.

**Researchers to conduct reliable studies** with larger sample sizes and greater confidence in their findings. Academic institutions have reported that studies using OpenSecrets data now require significantly less time for data preparation, allowing more resources to be devoted to analysis and interpretation.

**Citizens to make informed decisions** about political candidates through more accessible and comprehensive information. The improved data powers user-friendly tools on the OpenSecrets website, including interactive donor maps, contribution timelines, and influence network visualizations that make complex political finance data understandable to general audiences.

### **Scaling for future growth**

OpenSecrets' transformation supports democracy through **increased transparency**, while the system's scalability meets growing demands for political transparency tools as it continues to expand for new data sources. The AWS infrastructure can easily accommodate:

**State and local data integration**: The system is already processing campaign finance data from **15 additional states**, with plans to expand to all 50 states over the next two years. Each new data source requires minimal infrastructure changes, as the flexible architecture can accommodate varying data formats and filing requirements.

**Real-time processing capabilities**: During major election cycles, the system can provide **near real-time updates** as new filings are submitted to election commissions, enabling journalists to report on contribution patterns as they develop rather than waiting for quarterly summaries.

**International expansion potential**: The underlying technology stack could be adapted to process political finance data from other democratic countries, supporting global transparency initiatives and comparative political research.

### **Democratizing access to political data**

Perhaps most significantly, the transformation has **democratized access** to political finance information. Previously, only well-resourced news organizations or academic institutions could afford the staff time required to conduct comprehensive political finance research. Now, smaller news outlets, civic organizations, and individual researchers can access high-quality, processed data through OpenSecrets' APIs and bulk download services.

This democratization has led to a **proliferation of transparency initiatives** at local and state levels, as community organizations can now easily analyze their local political funding patterns and hold elected officials accountable for their funding sources.

---

## **Lessons for nonprofit technology implementation**

OpenSecrets' experience offers valuable guidance for other nonprofits embarking on technology transformation projects. The leadership team's first piece of advice is to **be flexible**, as the original plan might not be the best path once you're deep in the work.

### **Embrace iterative development over perfect planning**

The OpenSecrets transformation demonstrates the value of **agile methodology** in nonprofit technology projects. Rather than spending months creating detailed specifications, the team started with a **minimum viable product (MVP)** approach that could process a subset of their data and provide immediate value.

**Jacob Hileman**, OpenSecrets' IT Director, explains: *"We learned that trying to solve everything at once was actually slowing us down. By focusing on getting one piece working really well first, we could validate our approach and build confidence with stakeholders before tackling more complex challenges."*

This iterative approach allowed the team to:
- **Test assumptions early** with real data rather than theoretical scenarios
- **Build stakeholder buy-in** by demonstrating concrete results quickly  
- **Identify unforeseen challenges** before they became major roadblocks
- **Adapt to changing requirements** as the organization's needs evolved during the project

### **Prioritize explainability over sophistication**

The OpenSecrets team also emphasizes the importance of **building with users in mind**. The team needed **clear, explainable match logic**, not a black box solution. This user-centered approach led to the development of a final system that could be **trusted and effectively utilized** by OpenSecrets staff and external partners.

**Trust building was crucial** because OpenSecrets' reputation depends on data accuracy and transparency. Staff members needed to understand how matches were made so they could explain methodology to journalists, researchers, and the public. External users needed confidence that the underlying algorithms were sound and free from bias.

The decision to abandon machine learning in favor of **rule-based matching** exemplifies this principle. While ML approaches might have achieved slightly higher accuracy in some cases, the deterministic system provided:
- **Complete auditability** of every matching decision
- **Adjustable parameters** that domain experts could fine-tune
- **Explainable results** that could be defended in public forums
- **Reproducible processes** that external researchers could validate

### **Leverage cloud infrastructure for flexibility**

Perhaps most importantly, OpenSecrets learned **not to wait for perfection**. The team recommends **launching, learning, and refining iteratively**. Running everything on AWS made it easier to **pivot quickly** without re-architecting their entire system, enabling them to adapt their approach based on real-world testing and feedback.

**Cloud-first architecture** proved essential for managing uncertainty in the project scope. As the team learned more about their data challenges, they could:
- **Scale resources up or down** based on processing needs without capital investment
- **Experiment with different services** (Elasticsearch, various database engines, etc.) without infrastructure commitments
- **Deploy updates rapidly** through automated CI/CD pipelines
- **Rollback changes** quickly if new approaches didn't work as expected

### **Plan for change management and user adoption**

One unexpected lesson involved **change management** within the organization. Staff members who had spent years developing expertise in manual data processing initially viewed the automated system with skepticism. The team learned that **user buy-in** required more than just technical success—it needed careful communication and training.

Successful adoption strategies included:
- **Involving staff in algorithm development** by having them validate match results and suggest improvements
- **Training sessions** that helped staff understand how to use the new tools effectively
- **Gradual transition** rather than abrupt replacement of existing workflows
- **Clear documentation** that helped staff troubleshoot issues independently

### **Build partnerships and leverage grant opportunities**

The **AWS Imagine Grant** proved crucial not just for funding, but for creating accountability and external validation of the project's importance. The grant application process forced the team to clearly articulate their goals and success metrics, while the public nature of the grant created positive pressure to deliver results.

Other nonprofits should consider:
- **Applying for multiple grants** to diversify funding and reduce risk
- **Building relationships with technology partners** who understand nonprofit constraints
- **Documenting successes** to support future grant applications and inspire other organizations
- **Sharing lessons learned** with the broader nonprofit technology community

### **Measure impact beyond technical metrics**

While technical metrics like processing speed and accuracy were important, OpenSecrets learned to also measure **mission impact**. The true success of the project wasn't just in the technology itself, but in how it enabled the organization to better serve democracy.

Key impact measures included:
- **Increased media coverage** using OpenSecrets data in investigative stories
- **Academic research publications** leveraging the improved datasets
- **Citizen engagement metrics** through website usage and API adoption
- **Policy discussions** informed by more comprehensive political finance analysis
- **Organizational capacity** freed up by automation to focus on higher-value work

---

## **Key takeaways for organizations**

- **Flexibility is crucial**: Be prepared to adapt your original plan as you learn more about your challenges
- **User-centered design**: Build solutions that your team can understand, trust, and effectively use
- **Iterative approach**: Launch early, learn from real-world usage, and refine continuously
- **Cloud infrastructure advantage**: AWS enabled quick pivots and scaling without major re-architecture
- **Human-AI collaboration**: The best solutions enhance rather than replace human expertise

---

## **Supporting democracy through technology**

Democracy requires **political accountability**, which can only be achieved through **transparency**. OpenSecrets supports these core tenets of our political system by providing comprehensive and reliable data, analysis and tools for policymakers, storytellers, and citizens. The organization's transformation demonstrates how cloud technology can amplify the impact of nonprofit missions, creating more **accessible and accurate information** for democratic participation.

### **Expanding the transparency ecosystem**

The enhanced system empowers **researchers**, **journalists**, and **citizens** alike to better understand the flow of money in politics, ultimately contributing to a more informed and engaged democratic society. But the impact extends far beyond individual users—it's creating a **network effect** that strengthens democratic institutions.

**Academic institutions** are now incorporating OpenSecrets data into **curriculum development**, teaching students to analyze political finance patterns as part of civics and political science education. Universities report that students can now complete meaningful research projects using political finance data, whereas previously such projects required graduate-level resources.

**News organizations** are developing **automated alerts** that notify reporters when unusual contribution patterns emerge, enabling more timely coverage of political finance stories. Local news outlets, in particular, have benefited from the ability to quickly analyze their representatives' funding sources without requiring dedicated data teams.

**Civic technology organizations** are building **downstream applications** that make political finance data even more accessible to general audiences. These include mobile apps that let citizens scan QR codes on political advertisements to see funding sources, browser extensions that add contribution information to news articles, and social media bots that provide context on political spending in real-time.

### **Global implications for democratic transparency**

The success of OpenSecrets' transformation has attracted attention from **international transparency organizations** seeking to improve political finance oversight in other democracies. Several countries are now exploring similar approaches to automate campaign finance analysis, potentially creating a **global network** of political transparency initiatives.

**The European Union** has expressed interest in adapting the OpenSecrets methodology for cross-border political finance tracking, particularly given concerns about foreign influence in elections. The technical architecture developed with AWS could potentially scale to handle multi-jurisdictional data with different languages and regulatory frameworks.

**Emerging democracies** are particularly interested in the cost-effective approach, as many cannot afford large teams of data analysts but need effective tools to monitor political finance compliance and detect corruption patterns.

### **Technology as a force for democratic renewal**

The OpenSecrets transformation illustrates how **strategic technology investments** can strengthen democratic institutions at a time when trust in government and media is declining. By making political finance data more accessible and reliable, the project contributes to several important trends:

**Data-driven journalism** is becoming more prevalent as reporters gain access to better tools and datasets. This shift toward evidence-based reporting helps counter misinformation and provides citizens with more factual foundation for political discussions.

**Citizen oversight** capabilities are expanding as individuals and grassroots organizations gain access to tools previously available only to well-funded institutions. This democratization of oversight tools creates more distributed accountability mechanisms.

**Academic research** on political finance is accelerating as researchers can focus on analysis rather than data collection and cleaning. This is leading to better understanding of how money influences political outcomes and more evidence-based policy recommendations.

### **Future directions and sustainability**

Looking ahead, OpenSecrets plans to leverage its AWS infrastructure for several **innovative expansions**:

**Predictive analytics** could help identify potentially illegal contribution patterns or coordination between supposedly independent political actors. By analyzing historical patterns, the system might flag unusual activity for human investigation.

**Network analysis tools** will map complex relationships between donors, candidates, and political organizations, revealing influence networks that might not be apparent from individual contribution records.

**Integration with lobbying data** will provide more comprehensive pictures of organizational influence by connecting campaign contributions with lobbying expenditures and policy outcomes.

**Real-time monitoring** during election cycles could enable immediate detection of campaign finance violations or coordination between candidates and outside spending groups.

### **The broader nonprofit technology movement**

OpenSecrets' success contributes to a growing movement of **technology-enabled nonprofit innovation**. The project demonstrates that with appropriate cloud infrastructure and strategic partnerships, relatively small organizations can achieve impacts previously requiring much larger resources.

This has implications for the entire nonprofit sector, suggesting that **technology investments** should be viewed not just as operational improvements but as **mission multipliers** that can exponentially increase organizational impact. The AWS Imagine Grant program and similar initiatives are helping create a new generation of technology-sophisticated nonprofits that can leverage data and automation to serve their communities more effectively.

The ultimate measure of success isn't just in the technical achievements, but in the **strengthened democratic discourse** that results from more transparent and accountable political systems. By making political finance data more accessible, accurate, and actionable, OpenSecrets is contributing to a more informed citizenry and more responsive democratic institutions.

