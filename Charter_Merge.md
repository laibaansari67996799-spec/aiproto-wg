# Agent Communication Protocols (acp) Proposed Charter

The Agent Communication Protocols (acp) Working Group will work on defining protocol building blocks for enabling interoperability for AI Agent applications across the Internet. An AI agent is an autonomous, adaptive intelligent software system that uses AI models to complete a specific task. AI Agents often interact with users via chat or voice, performing tasks based on the flow of the conversation. To complete tasks on behalf of a human user or another AI agent, they can independently make decisions, execute actions, and interact with other AI agents and tools.

With the expansion of communication over the Internet between AI Agents and external resources that can be tools or other AI Agents, reliable communications across platforms and vendors becomes increasingly important. The role of the acp working group is to facilitate such interoperability so that tools and agents can be provided by multiple vendors.

# Key Considerations

There are several considerations that are unique to AI Agent applications that need to be addressed while working on developing the building blocks:

- AI Agents act as autonomous software entities that may need to be authenticated independently of the users they represent. Establishing verifiable agent identity that is distinct from user identity enables independent revocation of agent access, scoping of agent permissions to a subset of user permissions, and auditability of agent-initiated actions distinct from user-initiated actions.

- AI Agents also possess unique and differentiated skills that play a significant role in supporting collaborative tasks, which brings new considerations for how these specialized capabilities can be leveraged to select AI agents, initiate communication, and facilitate cross-domain interactions even within enterprise networks and on the Internet.

- Interactions of AI Agents with users, other AI Agents, and tools can be long-lived, utilize significant amounts of context across various modes (text, audio, video), and require very low latency (including fast barge/interruption times). This introduces new considerations around reliability, transport session management, and data transport.

- To protect data exchanged between AI Agents (and between AI Agents and tools) over potentially untrusted networks, particularly when handling sensitive information (such as personal data or conversational context), mechanisms are required to establish and verify identity, ensure confidentiality, integrity, authenticity of the exchanged data, selective disclosure and delegated authorization across AI Agent chains. This introduces new considerations around protocol-level security and privacy mechanisms.

Since AI agents can operate independently, and their LLMs can hallucinate, including the hallucination of tool use. This introduces risk when an AI Agent erroneously performs actions on a user's behalf. Techniques for mitigation—primarily through verifiable user approvals are critical.

The scope of the working group is agent-to-agent and agent-to-tools communication protocols. The working group will document common use-cases to derive requirements for the protocol.

# Deliverables

The working group will produce the following standards track and informational documents:

## AI Agent Session Protocol (Standards Track)

A standards-track protocoll for creation and maintenance of long-lived sessions between AI agents, or between AI agents and tools. These sessions allow for the bidirectional exchange of data, incuding model context, tool call results, and chat messages.

The session protocol will:

* Provide timed (short or long-lived) session management, enabling the establishment, update, context handling, and termination of the services of interacting agents and tools.
* Facilitate highly scalable and reliable session management, capable of surviving network and server failures while supporting graceful recovery.
* Support concurrent exchange of real-time data (such as voice and video), semi-real-time data (such as chat), and non-real-time data (such as tool call inputs and outputs).
* Supports point-to-point, point-to-multipoint and group communication topologies.

This protocol is a expected to be a foundational building block on top of which additional protocols can be built. It is anticipated that the AI Agent session protocol will utilize modern IETF application transfer protocols, such as QUIC, webtransport, WebRTC or MOQ, based on the anticipated use cases. The protocol must also be usable by other application layer protocols with the appropriate layering and extension points enabling its adoption by any application. Examples of protocols that can utilize this include the existing defacto agent communication protocols such as the MCP and A2A protocols being worked on by the Linux Foundation.

## AI Agent Authorization and Token Exchange Protocol (Standards Track)

A protocol that allows an AI Agent to obtain and exchange authorization tokens with fine-grained, behavior-driven scopes bound to the specific operations that the AI Agent is permitted to perform on behalf of the user. Unlike traditional OAuth scopes which enumerate static resource permissions, agent authorization must account for dynamic behavioral boundaries, including conditional and context-dependent privileges that may vary across interactions.

Any extensions to OAuth protocol mechanisms required to support agent authorization (e.g., new grant types, token structures, or authorization data types) are expected to be developed within the OAuth working group. This deliverable focuses on defining the agent-specific integration profile: how existing and emerging OAuth mechanisms are composed and applied in AI agent scenarios, including the confirmation and evidence requirements described above. This deliverable will also address selective disclosure mechanisms that enable agents to reveal capabilities contextually, with cryptographic verification of presented claims independent of transport security. This includes privacy-preserving presentations that prevent correlation across different interaction contexts.

- A mechanism for enabling human users to confirm operations invoked by AI agents acting on their behalf. This confirmation operation is performed by the agent orchestration layer of the AI Agent and therefore protects against hallucination and unintended actions. It is capable of working across multiple use cases, including a single AI Agent, or when using multiple AI Agents connected by the Agent-to-Agent protocol. It can be used to confirm any kind of operation, which includes invoking a REST API, calling a tool via MCP, or transferring to another AI Agent. It must provide cryptographic attestation of what the user has confirmed, enabling non-repudiation and auditable evidence trails. It might be simply a feature of the protocol defined in 2.

## AI Agent Protocol Framework (Standards Track)

A standards-track framework document that identifies the key building blocks and defines the protocol suite for interoperable agent-to-agent and agent-to-tool communications. The document provides an architectural overview and highlights areas for subsequent protocol specification work.

This is an evolving work item that can proceed in parallel with the development of specific protocol deliverables associated with the identified architectural blocks. It will iteratively integrate both existing and currently missing protocol building blocks in successive steps, until all core modules are fully incorporated.

The framework will:

* Enable AI Agents to select and collaborate with other AI Agents on the Internet or intranet, deployed in various interconnected domains and ecosystems, to execute simple or complex tasks.
* Allow multi-modal collaboration using varied data formats such as text, images, video, audio, and structured data with exchange of multi-modal contexts.
* Describe the functional blocks, their relationships, and the mechanisms for structured, semi-structured, and multi-modal information exchange to support collaborative tasks across domains.
* Identify the protocol suite covering session management, transport, security, and identity building blocks.

## Use Cases, Gap Analysis, and Requirements (Informational)

Foundational work will be documented through a set of informational Internet-Drafts covering:

* **Use cases** focused on Agent-to-agent and Agent-to-tool communications, used to verify the suitability of the protocols developed.
* **Gap analysis and requirements** based on examination of existing de facto protocols implemented in open-source projects, from which necessary protocol requirements are derived.

# Coordination

This working group is expected to closely coordinate with other related IETF working groups:

* **Security:** Web Authorization Protocol (OAuth), webbotauth, WIMSE — on identity, authorization, and security considerations.
* **Transport:** WebTransport, MoQ, QUIC, TSVWG — on data transport and session management.
* **Discovery and Operations:** INT area, OPS area — on agent discovery and operational considerations.

If the working group needs any changes to or extensions of protocols specified by other working groups, those issues will be raised with the relevant working groups for decisions on how best to handle them. The group is also expected to maintain close communication with open-source projects running under the Linux Foundation.


# Out of Scope

The following topics are explicitly out of scope for this working group:

- Discovery of AI agents
    
- Definition of AI models, agent reasoning algorithms, or tool-specific business logic.

- Standardization of agent behavior, decision-making, or planning semantics.

- AI agent behavioral security (e.g., preventing the AI model itself from hallucinating, though mitigating the impact of hallucinations via protocol-level user confirmation is in scope).

# TENTATIVE: (Depends on buy in from relevant open source communities)

x. Agent-to-Agent (A2A) Protocol (S)
An Agent-to-Agent protocol that allows one AI Agent to invoke the services of another AI Agent. This protocol will allow for the exchange of user messages, including voice, video, images, and chat. It will provide basic lifecycle management, enabling the establishment, update, and termination of the services of the downstream agent. It is anticipated that this protocol would build upon existing industry work, including the MCP and A2A protocols currently under the auspices of the Linux Foundation.
