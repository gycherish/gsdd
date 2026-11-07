# Requirements Specification

`docs/product/requirements.md` follows the outline of Karl Wiegers' software requirements specification template, which builds on IEEE 830, the predecessor of ISO/IEC/IEEE 29148. Keep the sections the product needs, in this order and with their numbers; leave out those that do not apply; and add any section the product needs. What each section says, and how, is your judgment.

| Section | Holds |
|---------|-------|
| 1 Introduction | 1.1 Purpose: the product, and the version this document specifies in full. 1.2 Document conventions: how versions are marked, and the other conventions the document uses. 1.3 Intended audience and reading suggestions. 1.4 Product scope: what the product is for, its benefits and goals. 1.5 References: the standards and the public documents outside the project that the requirements rely on. |
| 2 Overall description | 2.1 Product perspective: a new self-contained product or part of a larger system, and what it connects to. 2.2 Product functions: the feature set (below). 2.3 User classes and characteristics. 2.4 Operating environment: the platforms with their versions, and the software the product coexists with. 2.5 Design and implementation constraints: what limits the developers' options, the technical constraints the user states included. 2.6 User documentation. 2.7 Assumptions and dependencies. |
| 3 External interface requirements | 3.1 User interfaces: their logical characteristics. 3.2 Hardware, 3.3 software, and 3.4 communications interfaces: each connection to devices, other software, files, services, and networks, with the data that crosses it. |
| 4 System features | One section per feature planned for a version: its description and version, the sequences of user actions and system responses, and its functional requirements, with the responses to errors and invalid input. |
| 5 Other nonfunctional requirements | 5.1 Performance, 5.2 safety, 5.3 security and privacy, 5.4 other quality attributes, and 5.5 business rules. These are all the product's quality requirements: each states its measure and the conditions under which it holds, so that it can be checked. |
| 6 Other requirements | The requirements that no section above holds. |
| Appendix A: Glossary | The product's terms, each with its meaning. |
| Appendix B: Analysis models | The diagrams that explain the requirements as a whole: data, states, or flows. |

The template's revision history and its list of points to be determined are left out: git keeps the history, and open points are put to the user (`shared/principles.md`).

## The Feature Set

Section 2.2 holds the whole feature set: the features of the products of the same kind and the needs the research found, merged into one set, and grouped by the areas users see. Each feature has:

- its name (`shared/documents.md`);
- its version: `MVP` for the first release, a later version by the name the user gives it, or none while it is not planned yet;
- its source: the products it comes from, the finding of the research behind it, or the user.

A feature not planned yet is described there in one or two sentences. A feature planned for a version also has its section in 4.
