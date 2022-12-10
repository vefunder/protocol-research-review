


# veFunder Bounty Program

## **Summary**

We propose to create a Curve Community Bounty Program, beginning with a Gauge Risk focus area. This will replace the previous salary model. We will use a Dework board to manage the status of tasks, and create a veFunder multisig to manage the bounty payments.

## **Background**

In December 2021, we began recruiting members of the community to form a [Gauge Risk Team](https://cryptorisks.substack.com/). Their task has been pretty clearly defined: help give guidance to the DAO during new gauge proposals by assessing the risks of the applicant protocol, and working with those teams to meet our safety expectations.

This has so far been a successful endeavor. Team members have produced a number of reports that many in the community appreciate and use for guidance. The increased awareness of relevant risks has helped the DAO make informed decisions. It has also helped form stronger partnerships with applicant protocols, who appreciate having a dialogue and an interest taken in helping them improve. 

We have extended grants to 3 people to the Gauge Risk team (Fiddy, Amadeo, and Chan-ho), and are assisted by a community contributor who we hope to bring on board (evmknows). One of the members (Fiddy) has even been brought on as a Curve core team member, demonstrating that this program can be useful for engaging talented community members to help build Curve. 

However, the salary model for hiring members has presented a scaling challenge. We have limited manpower, often finding ourselves in the position of facing a large number of incoming proposals, and only being able to address one or two.

For that reason, we would like to explore a new model for attracting talent from the community that can scale to support more members and address a larger number of incoming proposals. This new bounty model can begin by focusing on Gauge Risks, but can expand to cover a wide range of community led focus areas.

## **Communications**

The foundation of the Gauge Risk team is its communication medium, and in that regard, the bounty program would operate much as it does today. We mostly discuss together in a Gauge Risk Telegram group, with some external communication with gauge applicants in private or public chats on Discord and Telegram.

With a transition toward a bounty program, our communication channels will become more public, and likely shift more toward Discord. The #veFunder Discord server spearheaded by Fiddy already has a number of active community members, and much of the public discussions will move there. We will still use the TG group since some members (read: Mich) only use TG, and we will keep this chat more as an internal team chat.

The Gauge Risk team has built a good reputation thanks to its high quality of work and the reputation of its members. Therefore, while discussions will be held publicly, there will be a nomination process for allowing new members to lead risk assessments and receive bounties for their work. 

## **Task Manager**

[Dework](https://app.dework.xyz/) is a task management tool for DAOs and web3 projects. It lets users create an organization that is composed of any number of focus areas or projects. In our case, our veFunder Bounties organization will begin with a Gauge Risk project, and eventually expand to others as the need arises.

Sample view of organization composed of several projects (sample Dework board is at [https://app.dework.xyz/i/6nUAjVPwKwgNzdP3QYJ6OF)](https://app.dework.xyz/i/6nUAjVPwKwgNzdP3QYJ6OF)):



![alt_text](https://github.com/vefunder/protocol-research-review/blob/main/admin/risk_budget_proposal_2022/images/image2.png)



Admins of the group can create and manage tasks represented as descriptive cards. Members can login, submit work for their assigned tasks, and accept payment for their work via wallet providers like Metamask. 

Sample view of Dework board (notice bounties are assigned with task cards):



![alt_text](https://github.com/vefunder/protocol-research-review/blob/main/admin/risk_budget_proposal_2022/images/image1.png)


Tasks and projects are managed by a group admin, who can also assign permissions to other members. In our case, we propose Wormhole and Fiddy will admin. We are also open to having another Curve team or grant council member admin, if requested.

Sample view of task creation:


![alt_text](https://github.com/vefunder/protocol-research-review/blob/main/admin/risk_budget_proposal_2022/images/image3.png)


There are several ways to assign a group member to a task. In our case, admin will assign members to tasks, possibly transitioning to Discord role assignment (members with specified discord roles can assign themselves to a task, while others must apply).

## **Bounties**

Dework tasks can be assigned a bounty in CRV. When the work has undergone review (admin has approved completion of the task), the bounty can be sent to the member assigned on the task. Payment methods are either by EOA or gnosis safe, and multiple payment methods can be added to the group. Gnosis safe is the preferred method of payment, as it is more secure and supports batch transactions.

Sample view of creating gnosis batch txn for recently approved bounties:



![alt_text](https://github.com/vefunder/protocol-research-review/blob/main/admin/risk_budget_proposal_2022/images/image4.png)


We plan to assign bounties for gauge risk assessments at a price of 500 - 1000 CRV, depending on the demands of the task. The Gauge Risk team has historically averaged about 3 research reports per month, whereas Curve gauge votes have averaged about 14 per month. Using the proposed bounty prices, we could easily 2-3x the number of proposals the team can cover at the current budget (currently 4000 CRV per month).

In addition to full risk reports, there are small tasks that may be required. For instance, some cases may only require an hour of research and tweet thread. In other cases, the responsibility lies most in being a point of contact with a particular team, rather than submitting a report. These bounties might involve submitting updates on the status of that team and the communications. 


<table>
  <tr>
   <td><strong>Bounty Type</strong>
   </td>
   <td><strong>Price (CRV)</strong>
   </td>
  </tr>
  <tr>
   <td>Large Risk Report, including comms. with applicant team, 2 researchers
   </td>
   <td>1,000
   </td>
  </tr>
  <tr>
   <td>Small Risk Report, 1 researcher
   </td>
   <td>500
   </td>
  </tr>
  <tr>
   <td>Research w/ tweet thread
   </td>
   <td>100
   </td>
  </tr>
  <tr>
   <td>Point of Contact + internal updates
   </td>
   <td>100/week * # of weeks
   </td>
  </tr>
</table>


These bounty prices are suggestions to begin with, but will likely be revised after some experience with the process and fluctuations in the CRV/USD price.  

## **Multisig**

Gnosis safe address: 0xa2482aA1376BEcCBA98B17578B17EcE82E6D9E86

We propose to use a multisig specific to the Community Bounty Program, composed of tenured members of the Gauge Risk team. This multisig will be responsible for making monthly bounty distributions. Multisig signers are Wormhole, Fiddy, Amadeo, Chan-ho, and Naga with a 3/5 signer policy. We prefer to create a new multisig rather than use the grant council multisig for bounty payments because it will help enforce the program budget and alleviate the grant council from micromanaging this project. 

## **Future Projects**

Naga has been conducting research for Curve pool simulations, largely on his own. His experience has led to some development ideas that it may become appropriate for him to manage. For example, there has been some discussion to create a pool balance notification system that can assist with assessing the state of pools and alert when some action should be taken. Tools and Analytics could become a project that Naga manages.

UI Development for various Curve services could be a useful project. Examples might include development of an alt governance UI that improves the viewing and creation of DAO votes and the content of those votes. Another idea might be a Curve Grants UI to advertise the grants program and the types of projects it is looking to fund. 

## **Requested Budget**

We are requesting **40,000 CRV in total **for a budget that will cover us though EoY 2022. The monthly breakdown accounts for expected program expansion beyond Gauge Risk to additional focus areas.


<table>
  <tr>
   <td><strong>Month</strong>
   </td>
   <td><strong>Monthly Budget (CRV)</strong>
   </td>
  </tr>
  <tr>
   <td>June
   </td>
   <td>4,000
   </td>
  </tr>
  <tr>
   <td>July
   </td>
   <td>5,000
   </td>
  </tr>
  <tr>
   <td>August
   </td>
   <td>5,000
   </td>
  </tr>
  <tr>
   <td>September
   </td>
   <td>6,000
   </td>
  </tr>
  <tr>
   <td>October
   </td>
   <td>6,000
   </td>
  </tr>
  <tr>
   <td>November
   </td>
   <td>7,000
   </td>
  </tr>
  <tr>
   <td>December
   </td>
   <td>7,000
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Total: 40,000
   </td>
  </tr>
</table>


 

