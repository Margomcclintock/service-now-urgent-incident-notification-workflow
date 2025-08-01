# Incident Notification Fix - Uber Network Infrastructure

## System Overview

**Company Context:**
You're a ServiceNow Administrator at Uber, where reliable network infrastructure is critical for maintaining ride-hailing services across hundreds of cities worldwide.

**Your Role:**
As a junior ServiceNow admin on the IT Operations team, you've been tasked with fixing a critical gap in the incident management system that supports Uber's network infrastructure.

**Why This Matters:**
Last month, a critical network outage in the San Francisco data center went unnoticed for 2 hours because the notification system wasn't working properly, causing widespread service disruptions and affecting thousands of riders and drivers. The incident resulted in significant revenue loss and regulatory scrutiny.

**The Scenario:**
Your manager discovered that critical network incidents aren't triggering email notifications to the Network Operations team. Yesterday, someone created a Critical priority incident for a network outage, but no notifications were sent to the team. The system was supposed to automatically alert engineers within minutes of a critical incident being created.

**Your Task:**
Investigate and fix whatever is preventing the notification system from working before the next critical incident occurs. The goal is to make sure urgent network incidents trigger immediate email notifications to IT engineers, helping prevent SLA breaches.

---

## Implementation Steps

To properly configure the flow, I first identified the areas that were preventing the email from reaching the Network Operations team. The original flow didn’t have the correct specifications. It attempted to send an email when an incident was created, but the conditions weren’t scoped to **Critical priority** or incidents in the **Network** category. Additionally, the notification record was incorrectly set to route the message to an unrelated group. Lastly, the email body was blank.

### To Fix:

I corrected the implementation so SLAs won’t be missed for future critical networking events. First, I updated the **flow trigger** to include the condition:
`Category is Network AND Priority is Critical`.
Using **AND** ensures that both conditions must be met before the flow is triggered.

After setting the correct trigger, I updated the **action** to "Send Notification," specifically as an **email**.

Next, I opened the **Notification record** to verify all the settings were correct:

* Ensured the **table** was set to **Incident**
* Confirmed the **Active** checkbox was selected
* Reviewed all three core tabs:

  * **When to Send**: Set to **Triggered**, since it’s being launched by the flow
  * **Who Will Receive**: Set to the **Network Operations group**
  * **What It Will Contain**: Added a meaningful message body for the email

### Testing the Fix

To test, I created a new incident assigned to the Networking group and set the priority to Critical. After submitting, I checked the **Email Outbox** in ServiceNow and confirmed that the Network Operations team received the notification **within seconds** of incident creation.

---

## AI Scenario Analysis

### Hypothetical Scenario:

Your company operates globally across multiple time zones. Critical incidents often occur outside business hours when senior engineers aren't available. The current escalation process follows a rigid hierarchy that doesn't account for engineer expertise, current availability, or incident complexity, leading to prolonged resolution times.

### Analysis:

In this hypothetical scenario, it seems the company has a monolithic architecture, as the prompt indicates the escalation process doesn’t account for expertise level, availability, or complexity. Specifically, the system’s inability to evaluate incident complexity and availability during the escalation process further speaks to a vertically scaled application. In a vertically scaled application, the escalated notifications process for critical incidents is handled by one system that is “scaled up” to manage higher volume. For example, more resources are added to the same server, which eventually leads to the problems outlined in this scenario. As the company grows, the volume of incident tickets will increase, which will make the system lag and lead to prolonged resolution times.

Theoretically, the company could increase the memory or processing power continuously for the server hosting the escalation process; however, there is a limited scalability ceiling. This means the company will continue to have this problem as it grows. Vertically scaled applications lack flexibility, are less resilient due to the risk of single-point failure, and are best suited for small or medium load environments.

As a consultant for this company, I would solve the engineer expertise, current availability, incident complexity, and prolonged resolution time issues by switching to an agentic architecture. Agentic architectures would detect unresolved critical tickets and escalate them using AI agents that make context-based decisions. Each AI agent would operate like a microservice, meaning each agent would be responsible for an independent service.

For example, one AI agent could detect which responders are available using the company’s on-call schedule. Another agent could monitor Slack or Teams to check if someone is active, which adds another layer of real-time availability detection. Multiple agents would work independently and have their own channels to resolve the same issue, which also provides failover. There could also be an AI agent that pulls data from an incident response database to provide additional contact info for team members or escalation levels specific to certain incidents. Once the agents are created, a Large Language Model (LLM), such as ChatGPT, can be used to drive the decision-making across those agents.

To address challenges with global operations and time zone differences, AI agents could also factor in engineers' time zones and active working hours to avoid escalating incidents to unavailable responders. In addition, AI agents could analyze current workloads such as ticket queues or open tasks to avoid overloading already-busy engineers. Over time, the system can learn from past incidents, including who resolved similar issues most efficiently, to continuously improve future escalation paths.

In addition, having many agents working together allows for prioritizing based on impact, not just static priority labels. For example, the AI could distinguish between a standard Priority 2 ticket and a Priority 2 incident that affects 8,000 users and is trending on social media. In that case, the model could escalate it to a Priority 1.

An AI architecture can make intelligent escalations automatically or semi-automatically, depending on the level of control the company wants to keep. Lastly, AI agents learn over time, so they can improve and escalate differently in the future based on what they’ve learned from past outcomes.


