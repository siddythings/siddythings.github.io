---
author : "sid"
title : "What is a Dead Letter Queue (DLQ)?" 
# date : "2020-09-04"
# description : "In this part will take care of your HTML files"
summary: A Dead-Letter Queue (DLQ) is like a message timeout corner in a software system.
tags : [
    "aws",
    "sql",
    "lambda",
]
categories : [
    "AWS",
    "SQS",
]
weight: 99
# series : ["Flask Learing Guide"]
# aliases : ["flask-learing-guide"]
cover:
  image: /images/dlq.png
  hiddenInList: true
---

## What is a Dead-Letter Queue (DLQ)?

A Dead-Letter Queue (DLQ) is like a message timeout corner in a software system. It holds messages temporarily that the system can't process due to errors. Imagine it as a safety net for messages that need a little extra attention.

### Why are DLQs Important?

Preventing Overflow: Just like a goalie in a soccer game, DLQs stop the main queue from being flooded with messages that the system can't score with.

Two Causes for DLQ Entry:
    Erroneous Content: It's like sending a secret message, but the courier accidentally spills coffee on it during delivery.
    Changes in Receiver's System: The system has a makeover, but the sender isn't informed. It's like trying to send a letter to a friend who moved without telling you.

### Benefits of a Dead-Letter Queue

Reduced Costs: DLQs are like bouncers at a club; they swiftly handle unruly messages instead of letting them cause chaos on the dance floor.

Improved Troubleshooting: Think of DLQs as a detective's board. Move problematic messages there, and your developers can focus on solving the mystery behind why messages aren't making it to the destination.

### When to Use a Dead-Letter Queue?

Use a DLQ when:

    Your system has queues that party without a specific order.
    FIFO queues are in use, but your DLQ follows the same "first come, first served" principle.

### When Not to Use a Dead-Letter Queue?

Avoid using DLQs:

    With queues that insist on endless retries, like a friend who keeps asking if you're free even when you're clearly not.
    With FIFO queues for things like editing videos. Changing the order could turn your action movie into a romantic comedy.

### How Does a Dead-Letter Queue Work?

    Creating a Redrive Policy: It's like setting rules for a board game. The redrive policy decides when to kick a message out of the main game and into the timeout corner (DLQ).

#### Moving Messages In:
    Failed delivery attempts are like missed goals. The ball (message) didn’t reach the intended destination.
    Reasons include messages lost in transit, messages throwing errors, or messages being too big to fit in the goal (queue).

#### Moving Messages Out:
    Developers put on their detective hats, figuring out why messages misbehaved.
    After fixing issues, messages return to the main game, ready to score points.
