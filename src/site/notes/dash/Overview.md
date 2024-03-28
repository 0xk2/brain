---
{"dg-publish":true,"permalink":"/dash/overview/"}
---

# Concept
---
There are two important concepts in DASH: `workflow`, `mission` and `checkpoint`.
Each `workflow` is a tree of `checkpoint`. Each `checkpoint` defines several key pieces of information:
- What is the rule for the decision within the checkpoint to be made?
- What are other checkpoints that link to this one?
A `workflow` is a class and a `mission` is an instance, like this pseudo code:

```
w = Workflow({...})
m = w.start({...})
m.vote({user, choice})
```

A `workflow` can be represented like this:
```
{
 start: 1,
 checkpoints: [
	 {id: 1, children:[2,3], vote_machine: "a_program_address"},
	 {id: 2, children: [4], , vote_machine: "a_program_address"}
 ]
}
```

A vote machine, an independent program that defines the rule for the decision within the checkpoint, can define the rule. For example, it could be a bridge to an `SPL governance` or `Squad` proposal, a snapshot or tally proposal, or even a `Web2 API` for which data will be provided by a provider. 

DASH exposes two functions to a vote machine:
- `move_next_checkpoint`
- `change_variables`

A DASH-votemachine interaction can be an one-leg flow like these two vote machine:

![Screenshot 2024-03-28 at 05.01.08.png](/img/user/dash/images/Screenshot%202024-03-28%20at%2005.01.08.png)

Or it can be a multiple-legs flow like this:

![Screenshot 2024-03-28 at 05.06.41.png](/img/user/dash/images/Screenshot%202024-03-28%20at%2005.06.41.png)
# System Design
---
There are three big components in DASH system:
- Entity: hold information on whom interacting with the system
- Workflow: hold information on Workflow, CheckPoint, Trigger
- Mission: hold information on Mission, VoteData, Variable, ProviderVariable

![Screenshot 2024-03-27 at 21.23.01.png](/img/user/dash/images/Screenshot%202024-03-27%20at%2021.23.01.png)

Let's go through each part:

## Entity
---
To create/publish a workflow, you must create an organisation and set `authority` for the organisation. The authority can be an individual public key, a Realm DAO, or a [Squad](squads.so). A workflow can only be created through a passed proposal if the authority is a Realm DAO or Squad.

![dash_entity.png](/img/user/dash/images/dash_entity.png)

Each user will hold a `User PDA` to store the number of missions they have created. This PDA would serve the convenience of listing all users' missions, saving time on paginating mission listing on the front end.

## Workflow
--- 
Each workflow will refer to the starting `checkpoint`; each checkpoint will hold the reference to its children.

![dash_workflow.png](/img/user/dash/images/dash_workflow.png)

## Mission
---
A `Mission` will be initiated from a `Workflow`, hold its `current_vote_data`.

![dash_mission.png](/img/user/dash/images/dash_mission.png)


