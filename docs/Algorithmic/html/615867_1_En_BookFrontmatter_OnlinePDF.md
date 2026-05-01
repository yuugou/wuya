Front Matter

Viktoria Dolzhenko

# Algorithmic Trading Systems and Strategies: A New Approach

## Design, Build, and Maintain an Effective Strategy Search Mechanism

![](../images/615867_1_En_BookFrontmatter_Figa_HTML.png)

The Apress logo.

Viktoria Dolzhenko

San Jose, Costa Rica

ISBN 979-8-8688-0356-7e-ISBN 979-8-8688-0357-4

[https://doi.org/10.1007/979-8-8688-0357-4](https://doi.org/10.1007/979-8-8688-0357-4)

© Viktoria Dolzhenko 2024

This work is subject to copyright. All rights are solely and exclusively licensed by the Publisher, whether the whole or part of the material is concerned, specifically the rights of translation, reprinting, reuse of illustrations, recitation, broadcasting, reproduction on microfilms or in any other physical way, and transmission or information storage and retrieval, electronic adaptation, computer software, or by similar or dissimilar methodology now known or hereafter developed.

The use of general descriptive names, registered names, trademarks, service marks, etc. in this publication does not imply, even in the absence of a specific statement, that such names are exempt from the relevant protective laws and regulations and therefore free for general use.

The publisher, the authors and the editors are safe to assume that the advice and information in this book are believed to be true and accurate at the date of publication. Neither the publisher nor the authors or the editors give a warranty, expressed or implied, with respect to the material contained herein or for any errors or omissions that may have been made. The publisher remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

This Apress imprint is published by the registered company APress Media, LLC, part of Springer Nature.

The registered company address is: 1 New York Plaza, New York, NY 10004, U.S.A.

_This book is dedicated to all those who are constantly in search of new ideas. These tireless adventurers are undeterred by any difficulties or failures and have been looking for something new all their life to improve both their lives and the lives of the people around them. I admire such people immensely. It is thanks to them that the world continues to improve at least a little bit._

_Dare and create. This is the purpose of life._

Introduction

If you start researching algorithmic trading, you will notice a general pattern in the logic of creating trading systems. That pattern is to find a few high-profit strategies and use them in the trading. Often the search is carried out using a lot of manual labor. There is one big drawback in this approach: there is a low probability that a profitable strategy will be found.

Also, because of the amount of manual labor in this process, excessive demands are placed on the strategies found. They require a lot of support and configuration.

I remember when I tried many different strategies, most of which showed low performance. I was increasingly frustrated with myself and the approaches I was using. One day, I remembered some work I did with a teacher at my university. We were looking for optimal parameters for launching and adjusting the movement of artificial earth satellites to launch them into the target orbit with the least amount of fuel consumed. I thought, how does this task differ from the task of finding a profitable strategy? I realized that they were the same, which meant my knowledge in that area could be applied in a completely new direction.

As a result, I combined my knowledge of creating high-load systems and the field of engineering to create a new approach for finding and using profitable strategies. The solution lies not in looking for dozens of highly profitable strategies. The solution is to quickly find hundreds of strategies, albeit with more modest results, with minimal manual labor.

In this book, I not only describe a system that continuously searches for and runs strategies but also show an option for implementing the fundamental parts of such a system using the .NET Framework. In this book you will learn how you can build a trading system that finds profitable strategies with a higher probability than with conventional approaches.

Any source code or other supplementary material referenced by the author in this book is available to readers on GitHub (<https://github.com/Apress>). For more detailed information, please visit <https://www.apress.com/gp/services/source-code>.

Table of Contents

[Chapter 1:​ Popular Approaches to Developing Trading Systems](615867_1_En_1_Chapter.xhtml)1

[Manual Trading](615867_1_En_1_Chapter.xhtml#Sec1)2

[Ready-Made Signals and Algorithms](615867_1_En_1_Chapter.xhtml#Sec2)2

[Signals](615867_1_En_1_Chapter.xhtml#Sec3)3

[Third-Party Algorithms](615867_1_En_1_Chapter.xhtml#Sec4)5

[Specialized Services](615867_1_En_1_Chapter.xhtml#Sec5)9

[Independent Creation of a Trading Platform](615867_1_En_1_Chapter.xhtml#Sec6)10

[Testing a Single Strategy](615867_1_En_1_Chapter.xhtml#Sec7)10

[Various Developer Approaches](615867_1_En_1_Chapter.xhtml#Sec8)12

[Hiring Third-Party Developers](615867_1_En_1_Chapter.xhtml#Sec9)12

[My Approach](615867_1_En_1_Chapter.xhtml#Sec10)13

[Summary](615867_1_En_1_Chapter.xhtml#Sec11)15

[Chapter 2:​ Introduction to Developing Trading Systems](615867_1_En_2_Chapter.xhtml)17

[General Theory](615867_1_En_2_Chapter.xhtml#Sec1)18

[Order Execution](615867_1_En_2_Chapter.xhtml#Sec2)21

[Margin and Leverage](615867_1_En_2_Chapter.xhtml#Sec3)24

[Composition of the Trading System](615867_1_En_2_Chapter.xhtml#Sec4)25

[Trading Theory](615867_1_En_2_Chapter.xhtml#Sec5)27

[Capital Management](615867_1_En_2_Chapter.xhtml#Sec15)39

[Risk Control](615867_1_En_2_Chapter.xhtml#Sec22)49

[Testing](615867_1_En_2_Chapter.xhtml#Sec29)58

[Performance Indicators](615867_1_En_2_Chapter.xhtml#Sec30)59

[Optimization](615867_1_En_2_Chapter.xhtml#Sec31)63

[Summary](615867_1_En_2_Chapter.xhtml#Sec32)67

[Chapter 3:​ Architectural Solution Part 1:​ Identifying the Requirements](615867_1_En_3_Chapter.xhtml)69

[Requirements Elicitation](615867_1_En_3_Chapter.xhtml#Sec1)71

[Signals](615867_1_En_3_Chapter.xhtml#Sec2)72

[First View of the Process](615867_1_En_3_Chapter.xhtml#Sec3)74

[Theory Generator](615867_1_En_3_Chapter.xhtml#Sec4)78

[Strategies Searching](615867_1_En_3_Chapter.xhtml#Sec5)82

[Selection and Forward Testing](615867_1_En_3_Chapter.xhtml#Sec8)97

[Selection of Financial Instruments](615867_1_En_3_Chapter.xhtml#Sec9)98

[Setting Up a Search for a Profitable Strategy](615867_1_En_3_Chapter.xhtml#Sec10)99

[The Logic of Searching for Profitable Strategies](615867_1_En_3_Chapter.xhtml#Sec11)100

[Real Trading](615867_1_En_3_Chapter.xhtml#Sec12)101

[Important Questions](615867_1_En_3_Chapter.xhtml#Sec13)103

[Life Cycle of a Position](615867_1_En_3_Chapter.xhtml#Sec14)103

[Capital Management](615867_1_En_3_Chapter.xhtml#Sec15)106

[Risk Control](615867_1_En_3_Chapter.xhtml#Sec16)107

[Scalability of Indicators](615867_1_En_3_Chapter.xhtml#Sec19)114

[Summary](615867_1_En_3_Chapter.xhtml#Sec20)116

[Chapter 4:​ Architectural Solution Part 2:​ Services and Subsystems](615867_1_En_4_Chapter.xhtml)117

[Microservice Architecture](615867_1_En_4_Chapter.xhtml#Sec1)118

[Kubernetes](615867_1_En_4_Chapter.xhtml#Sec2)122

[Subsystems](615867_1_En_4_Chapter.xhtml#Sec3)124

[Strategy Search Subsystem](615867_1_En_4_Chapter.xhtml#Sec4)125

[Processing Generators](615867_1_En_4_Chapter.xhtml#Sec5)126

[Queue](615867_1_En_4_Chapter.xhtml#Sec6)130

[Finite State Machine](615867_1_En_4_Chapter.xhtml#Sec7)134

[Concept of Theory Processing Steps](615867_1_En_4_Chapter.xhtml#Sec8)135

[Calculation of Subtheories](615867_1_En_4_Chapter.xhtml#Sec9)140

[Core](615867_1_En_4_Chapter.xhtml#Sec13)149

[Sandbox Exchange](615867_1_En_4_Chapter.xhtml#Sec14)150

[Real Trading Subsystem](615867_1_En_4_Chapter.xhtml#Sec15)151

[Integration with Exchanges](615867_1_En_4_Chapter.xhtml#Sec16)152

[Launch and Operation of Strategies](615867_1_En_4_Chapter.xhtml#Sec17)156

[Enabling and Disabling Strategies](615867_1_En_4_Chapter.xhtml#Sec18)158

[Checking the Type of Financial Instrument](615867_1_En_4_Chapter.xhtml#Sec19)160

[Master Data](615867_1_En_4_Chapter.xhtml#Sec20)167

[Summary](615867_1_En_4_Chapter.xhtml#Sec21)169

[Chapter 5:​ Technology Stack and Libraries](615867_1_En_5_Chapter.xhtml)171

[Choosing a Framework](615867_1_En_5_Chapter.xhtml#Sec1)172

[Application Architecture](615867_1_En_5_Chapter.xhtml#Sec2)172

[Spaghetti Code](615867_1_En_5_Chapter.xhtml#Sec3)173

[Clean Architecture](615867_1_En_5_Chapter.xhtml#Sec4)174

[Domain-Driven Design vs.​ Anemic Model](615867_1_En_5_Chapter.xhtml#Sec5)177

[Object Relational Mapper](615867_1_En_5_Chapter.xhtml#Sec6)178

[How to Use Dapper](615867_1_En_5_Chapter.xhtml#Sec7)179

[Migrations](615867_1_En_5_Chapter.xhtml#Sec8)184

[Finite State Machine](615867_1_En_5_Chapter.xhtml#Sec9)185

[Principle](615867_1_En_5_Chapter.xhtml#Sec10)186

[Hosted Service](615867_1_En_5_Chapter.xhtml#Sec11)189

[Backworker](615867_1_En_5_Chapter.xhtml#Sec12)207

[Summary](615867_1_En_5_Chapter.xhtml#Sec13)213

[Chapter 6:​ Optimization Algorithms](615867_1_En_6_Chapter.xhtml)215

[Formulation of the Problem](615867_1_En_6_Chapter.xhtml#Sec1)216

[Population Algorithms](615867_1_En_6_Chapter.xhtml#Sec2)217

[Genetic Algorithms](615867_1_En_6_Chapter.xhtml#Sec3)220

[Mutation Operators](615867_1_En_6_Chapter.xhtml#Sec4)222

[Crossover Operators](615867_1_En_6_Chapter.xhtml#Sec8)224

[Filtering Operators](615867_1_En_6_Chapter.xhtml#Sec16)228

[Selection Operators](615867_1_En_6_Chapter.xhtml#Sec25)234

[Restrictions](615867_1_En_6_Chapter.xhtml#Sec30)238

[Local Unconstrained Optimization Algorithms](615867_1_En_6_Chapter.xhtml#Sec39)249

[Summary](615867_1_En_6_Chapter.xhtml#Sec52)264

[Chapter 7:​ Implementation of Optimization Algorithms](615867_1_En_7_Chapter.xhtml)265

[General View](615867_1_En_7_Chapter.xhtml#Sec1)266

[Brute-Force Algorithm](615867_1_En_7_Chapter.xhtml#Sec2)270

[Getting Info](615867_1_En_7_Chapter.xhtml#Sec3)272

[Getting a Set of Values](615867_1_En_7_Chapter.xhtml#Sec4)277

[How to Use](615867_1_En_7_Chapter.xhtml#Sec5)282

[Genetic Algorithm](615867_1_En_7_Chapter.xhtml#Sec6)285

[Steps](615867_1_En_7_Chapter.xhtml#Sec7)286

[Getting Info](615867_1_En_7_Chapter.xhtml#Sec8)287

[Getting a Set of Values](615867_1_En_7_Chapter.xhtml#Sec9)294

[Initialization Step](615867_1_En_7_Chapter.xhtml#Sec10)303

[Mutation Step](615867_1_En_7_Chapter.xhtml#Sec11)306

[Filtering Step](615867_1_En_7_Chapter.xhtml#Sec12)309

[Breeding Step](615867_1_En_7_Chapter.xhtml#Sec15)314

[Test Functions](615867_1_En_7_Chapter.xhtml#Sec20)321

[SubTheory Example](615867_1_En_7_Chapter.xhtml#Sec21)322

[Summary](615867_1_En_7_Chapter.xhtml#Sec22)325

[Chapter 8:​ Implementation of the Core Module](615867_1_En_8_Chapter.xhtml)327

[Use Cases](615867_1_En_8_Chapter.xhtml#Sec1)327

[Context](615867_1_En_8_Chapter.xhtml#Sec2)331

[Update Candle Event](615867_1_En_8_Chapter.xhtml#Sec3)332

[Check Signals](615867_1_En_8_Chapter.xhtml#Sec4)335

[Strategy Model](615867_1_En_8_Chapter.xhtml#Sec5)337

[Calculate Signal](615867_1_En_8_Chapter.xhtml#Sec6)343

[Calculation of Indicators](615867_1_En_8_Chapter.xhtml#Sec7)351

[Average True Range (Atr)](615867_1_En_8_Chapter.xhtml#Sec8)357

[Process of Positions](615867_1_En_8_Chapter.xhtml#Sec9)362

[Process Bot (Lite Version)](615867_1_En_8_Chapter.xhtml#Sec10)364

[Process Steps](615867_1_En_8_Chapter.xhtml#Sec11)373

[Events](615867_1_En_8_Chapter.xhtml#Sec12)376

[Process Acts](615867_1_En_8_Chapter.xhtml#Sec13)390

[Summary](615867_1_En_8_Chapter.xhtml#Sec14)396

[Chapter 9:​ Approaches to Final Implementation](615867_1_En_9_Chapter.xhtml)397

[Binance Adapter](615867_1_En_9_Chapter.xhtml#Sec1)397

[Implementation](615867_1_En_9_Chapter.xhtml#Sec2)398

[Docker](615867_1_En_9_Chapter.xhtml#Sec3)403

[History](615867_1_En_9_Chapter.xhtml#Sec4)403

[Why Is This Needed?​](615867_1_En_9_Chapter.xhtml#Sec5)405

[Docker Components](615867_1_En_9_Chapter.xhtml#Sec6)406

[Launching the Application](615867_1_En_9_Chapter.xhtml#Sec7)408

[Kubernetes](615867_1_En_9_Chapter.xhtml#Sec8)415

[Components](615867_1_En_9_Chapter.xhtml#Sec9)416

[Pods](615867_1_En_9_Chapter.xhtml#Sec10)417

[Deployments](615867_1_En_9_Chapter.xhtml#Sec11)419

[Services](615867_1_En_9_Chapter.xhtml#Sec12)424

[Helm](615867_1_En_9_Chapter.xhtml#Sec13)426

[Summary](615867_1_En_9_Chapter.xhtml#Sec14)429

[Index](615867_1_En_BookBackmatter_OnlinePDF.xhtml#Ind1)431

About the Author

Viktoria Dolzhenko

![](../images/615867_1_En_BookFrontmatter_Figb_HTML.jpg)

A photo of Viktoria Dolzhenko.

My dream of becoming a programmer started the moment a computer first appeared in my home. As a child I loved programming and started regularly participating in programming competitions. And now, I have more than 10 years of experience in developing complex systems from scratch.

My professional passion is the complex design of various applications, while personally I am interested in algorithmic trading. To bring these two ideas together, I decided to build a system that would search for and find profitable strategies on its own. At that time, developing such a system for algorithmic trading was an interesting and difficult task.

Now I am sharing my findings and conclusions in this book so that everyone who wants to build their own algorithmic systems will not waste time creating ineffective systems. Readers can learn from my experience to gain the maximum benefits from their creations.

About the Technical Reviewers

Ganesh Harke

![](../images/615867_1_En_BookFrontmatter_Figc_HTML.jpg)

A photo of Ganesh Harke.

is a seasoned technology leader working with a multinational investment bank. Over the past 14 years, Ganesh has channeled his expertise in design, architecture, and development of scalable and low-latency systems into various industries.

In his current role as technical lead at Citibank, Ganesh is at the forefront of developing a cutting-edge e-commerce platform. His professional journey extends beyond Citibank, with previous impactful roles at esteemed organizations such as Barclays and Siemens. He has contributed to and thrived in dynamic environments at each juncture, leaving an indelible mark on projects ranging from intricate financial solutions to cutting-edge technological innovations.

Ganesh has a master’s degree in financial engineering and a bachelor’s degree in information technology and has passed all three CFA levels.

Ganesh is also a mentor who helps fellow professionals and colleagues with his technical expertise. Ganesh was recognized for his work inside and outside of his career network.

Vadzim Zylevich

![](../images/615867_1_En_BookFrontmatter_Figd_HTML.jpg)

A photo of Vadzim Zylevich.

is a certified Microsoft Solutions Developer with a strong background in software development, focusing on technologies like C# and ASP.NET. Over his 10-year career, he has led several important projects and has been recognized for his technical skills and leadership. Currently, he works at Maersk, the leading global logistics company, where he continues to apply his expertise to innovate and improve their technological capabilities. Beyond programming, he is a technical writer at Code-Maze and mentors aspiring developers, sharing his knowledge and experience. His interest in the impact of AI on software development led him to discuss this topic in a TickerNews interview, highlighting his commitment to both innovation and education in the tech field. His goal is to not only advance in technology but also support the growth of the tech community by making complex concepts accessible to all.

Samantha

![](../images/615867_1_En_BookFrontmatter_Fige_HTML.jpg)

A photo of Samantha.

As a senior portfolio performance and risk expert, **Samantha** delivers multi-asset expertise within her area and clarifies the strategic solutions to global asset managers and sell-side players leveraging Bloomberg’s suite of solutions and models. Specializing in liquidity stress assessment, fixed-income analytics, and customized indexing within the context of various investment vehicles, Samantha has a wealth of knowledge of the ever-shifting investment landscape and the relevant world-class solutions to address the regulatory and industry challenges.
