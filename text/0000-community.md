- Feature Name: community
- Start Date: 2018-03-13
- RFC PR: (leave this empty)
- Sawtooth Issue: (leave this empty)

# Summary
[summary]: #summary

The Sawtooth community encourages contributions to the project from all
interested individuals. This RFC explicitly defines the roles and
responsibilities of Contributors and Maintainers, as well as the process for
gaining and losing Contributor and Maintainer status.

# Motivation
[motivation]: #motivation

1. To help ensure the best interests of the project are protected and
   maintained
2. To promote an appropriate level of peer-review.
3. To clarify the responsibilities of community members.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

The Sawtooth community encourages contributions to the project from all
interested individuals. The project explicitly defines three levels of
community member status:

1. Community Member
2. Contributor
3. Maintainer

A *Community Member* is any individual who has interacted with the project.
There is no special process for becoming a Community Member.

A *Contributor* is a Community Member who is actively involved in supporting
the project by:

- Submitting improvements and bug-fixes
- Assisting other community members
- Submitting RFCs

A *Maintainer* is a Contributor who is a thought leader within some part of the
project. They are more involved in the project than a Contributor and support
the project by:

- Carefully reviewing pull requests
- Reviewing and voting on RFCs
- Setting the direction of the project
- Keeping the project up-to-date with the latest patches and security updates

The Sawtooth project encompasses a growing list of components. In order to
fairly distribute responsibility over these different components, _Contributor
and Maintainer status is on a per-component basis_. A group of Contributors and
Maintainers for a specific component form a "sub-team" for that component.
Teams can also form around specific tasks such as licensing.

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

The following more clearly defines the responsibilities of the different
community member statuses as well as codifying the process for gaining these
statuses.

## Community Member

Anyone who interacts with Sawtooth is a community member. This includes but is
not limited to:

- Downloading or cloning any component of Sawtooth
- Demoing Sawtooth
- Building an application with Sawtooth
- Interacting with other community members through the chat server or mailing
  list

Community Members do not have any responsibilities and there is no process for
becoming a Community Member.

## Contributor

Contributors are Community Members who have contributed to the project is some
way. The exact prerequisites for becoming a contributor will vary by sub-team
and is up to the Maintainers for that sub-team to define clearly. However, in
general Contributors should be anyone who has:

- Contributed code to Sawtooth through the pull request process
- Assisted other Community Members on the chat server or mailing list
- Provided constructive feedback on the project in the form of opening Bug
  Reports, reviewing pull requests, or providing providing detailed feedback
  on their experience using Sawtooth

### Responsibilities

Contributors have authorization to merge code into the project once it has gone
through the review process successfully and been approved. See
the [Contributing Guide](https://sawtooth.hyperledger.org/docs/core/releases/latest/community/contributing.html#commit-process)
for details on the pull request approval process.

### Becoming a Contributor

A Community Member becomes a Contributor when a Maintainer promotes them and
loses Contributor status when a Maintainer demotes them.

## Maintainer

Maintainers are Contributors with a higher level of authority and
responsibility within the project. They are thought-leaders within the
component of the project they are responsible for and help set the direction
for the project. Typically, Maintainers are Contributors who also:

- Carefully review pull requests and leave thoughtful, constructive feedback
- Socialize and contribute new RFCs
- Ensure the project is up-to-date with patches and security updates
- Promote a positive community around the project
- Participate in community events
- Help set project goals and guide the project to success

### Responsibilities

Maintainers have a set of responsibilities which must be met to ensure forward
progress of the project. These responsibilities include:

- Reviewing pull requests in a timely manner
- Reviewing and voting on RFCs
- Participating in Maintainer-level meetings
- Participating in discussions via the mailing list and chat server

### Becoming a Maintainer

In order to become a Maintainer, an individual must first become a Contributor.
Contributors can then become Maintainers through promotion by unanimous vote of
the current Maintainers.

Maintainer status can be lost due to:

- Inactivity of 6 Months
- Unanimous vote by all other Maintainers
- Violating the [Hyperledger Code of Conduct](https://wiki.hyperledger.org/community/hyperledger-project-code-of-conduct)

# Drawbacks
[drawbacks]: #drawbacks

This RFC codifies a hierarchy of community member statuses and processes for
attaining these different levels of status. Since this is new, some existing
community members may feel offended by not being given a status level that is
consist with their expectations.

# Rationale and alternatives
[alternatives]: #alternatives

Having a clearly defined community member status hierarchy and process for
moving through the hierarchy helps to set clear expectations for the community.
It helps avoid potential conflict and confusion resulting from unclear
expectations.

Having a flatter design is problematic because it makes it more difficult to
set project direction and build consensus around new ideas, features, and
improvements.

The process for moving between the different statuses is intentionally short
and left up to the Maintainers of each sub-team to define more fully. An
alternative to this process would be a careful thought out set of
qualifications before changing status. A short and simple process is preferred
because it allows the individual sub-teams to organize as they see fit and to
follow their own community norms, while still ensuring a minimal amount of
structure.

# Prior art
[prior-art]: #prior-art

The hierarchy proposed herein is loosely based on ideas from the [Hyperledger
Fabric Contributor
Guide](https://hyperledger-fabric.readthedocs.io/en/release-1.0/CONTRIBUTING.html)
and preexisting community norms within the Sawtooth project.

# Unresolved questions
[unresolved]: #unresolved-questions

- What parts of the design do you expect to resolve through the RFC process
  before this gets merged?
- What parts of the design do you expect to resolve through the implementation
  of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be
  addressed in the future independently of the solution that comes out of this
  RFC?
