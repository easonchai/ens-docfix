Hello ENS team!

I've been using you guys for the past 4 years, and it has been an absolute joy. This year, I decided to put a twist to my usual hackathon experience and decided to sponsor and mentor a bunch of new Web3 developers as a way for them to grow. Working closely with new developers during this hackathon, I've gathered substantial feedback regarding the ENS documentation. Many newcomers faced challenges while integrating ENS into their projects due to gaps and inconsistencies in the documentation. Below, I've detailed specific areas where the documentation could be improved to enhance the developer experience, especially for those who are new to blockchain development.

### 1. Clarify Multi-Chain ENS Resolution

**Relevant Pages:**

- [Deployments](https://docs.ens.domains/learn/deployments)
- [CCIP-Read](https://docs.ens.domains/learn/ccip-read)
- [Multichain](https://docs.ens.domains/web/multichain)

**Issues:**

- **Lack of Clarity on ENS Resolution Across Chains:** The documentation does not explicitly state that ENS names can be resolved on Ethereum mainnet or testnets regardless of the chain a developer is building on. Since ENS is deployed only on Ethereum mainnet, Sepolia, and Holesky, developers working on other chains (e.g., Base) might be confused about how to resolve ENS names. **This is not clear for new developers jumping into Web3!**
- **Configuration Challenges with Libraries:** Libraries like `viem` require separate configurations for interacting with ENS on Ethereum while performing other actions on a different chain. This separation is not clearly documented, leading to confusion. Example below:

```js
import { useAccount, useEnsName, useEnsAvatar } from "wagmi";

const Name = () => {
  const { data: ensName } = useEnsAddress({
    address: "luc.eth",
    chainId: 1, // As a new developer, I might try to use my own chain I am building on, and not either Mainnet, Sepolia, or Holesky.
  });

  return <div>{ensName || address}</div>;
};
```

**Suggestions:**

- **Explicitly Mention Cross-Chain Resolution:** Update the documentation to clearly state that ENS names are resolved on Ethereum mainnet or testnets, regardless of the chain used for development. It should be made explicit in all the links above wherever possible.

- **Provide Configuration Examples:** Include code snippets showing how to configure libraries like `viem` to interact with ENS on Ethereum while working on another chain.

- **Add a Chain Support Table:** Incorporate a table in the [Multichain](https://docs.ens.domains/web/multichain) page that lists supported chains, similar to how other projects present this information (e.g., [Magic Link](https://magic.link/docs/blockchains/overview), [Privy](https://docs.privy.io/guide/react/configuration/networks)).

Example:
| Network | Chain ID |
|----------|----------|
| Ethereum | 1 |
| Sepolia | 11155111 |
| Holesky | 17000 |

While you do have information on the deployments & their relevant addresses, this is not "beginner friendly" since they don't even know which chains can they use ENS on yet:
https://docs.ens.domains/learn/deployments

---

### 2. Enhance Subdomain Documentation with Code Examples

**Relevant Pages:**

- [Subdomains](https://docs.ens.domains/web/subdomains)
- [Name Wrapper](https://docs.ens.domains/wrapper/contracts)
- [Contracts](https://docs.ens.domains/contracts)

**Issues:**

- **No Guidance on Creating Subdomains:** The `Subdomains` page does not provide instructions or code examples on how to create subdomains programmatically.

- **Difficulty Finding Relevant Information:** Critical information about the `NameWrapper` contract and how to use it is buried deep in the documentation, making it hard for developers to find. Let me walk through the navigation flow:
  Start from [Intro](https://docs.ens.domains/learn/protocol) -> Looks at Subdomains -> Click the section & redirected to [Subdomains](https://docs.ens.domains/web/subdomains) page. However, this doesn't actually tell me how to create a subdomain. It provides no code to write, and not much information upfront that a fresh user can use, which is disorienting. Looking around, I see "Prebuilt Tooling", indicating existing tools or SDKs I can perhaps use, but it brings me to the [NameWrapper Overview](https://docs.ens.domains/wrapper/overview) page instead, which again doesn't have any code, but high level information. Considering its a developer doc, this can be frustrating for users. Same thing for Legacy Subdomains section
  <img width="839" alt="Screenshot 2024-09-22 at 5 52 22 AM" src="https://github.com/user-attachments/assets/ef5b4d20-87f8-4c6b-aa5d-89cb3e1b9253">

- **Missing References:** The `NameWrapper` contract is not referenced in the [Contracts](https://docs.ens.domains/contracts) page, leading to further confusion.

**Suggestions:**

- **Include Step-by-Step Guides:** Add clear instructions and code examples on creating subdomains using the `NameWrapper` contract directly on the `Subdomains` page.

- **Provide Sample Code:**

  ```javascript
  import { writeContract, parseAbi } from "viem";

  const NAMEWRAPPER_ADDRESS = "0x0635513f179D50A207757E05759CbD106d7dFcE8"; // Sepolia address

  const nameWrapperABI = parseAbi([
    "function setSubnodeRecord(bytes32 node, string calldata label, address owner, address resolver, uint64 ttl, uint32 fuses, uint64 expiry) public",
  ]);

  await writeContract({
    address: NAMEWRAPPER_ADDRESS,
    abi: nameWrapperABI,
    functionName: "setSubnodeRecord",
    args: [
      parentNode,
      label,
      owner,
      "0x0000000000000000000000000000000000000000", // Use default resolver
      0n, // TTL
      0, // No fuses
      0n, // No expiry
    ],
  });
  ```

- **Reference Contracts Appropriately:** Update the [Contracts](https://docs.ens.domains/contracts) page to include the `NameWrapper` contract and link to relevant sections.

- **Add Code Block Language Switcher:** Implement a dropdown on code blocks (as seen in other documentation pages) to allow developers to switch between different libraries like `ethers.js`, `viem`, or `wagmi`. Example in the image below:
  <img width="824" alt="Screenshot 2024-09-22 at 5 40 13 AM" src="https://github.com/user-attachments/assets/de275bdc-8114-4430-aea8-7cbedbdce957">

---

### 3. Improve Documentation Structure and Navigation

**Relevant Pages:**

- [Quickstart](https://docs.ens.domains/web/quickstart)
- [Tools & Libraries](https://docs.ens.domains/web/tools-and-libraries)
- [Subdomains](https://docs.ens.domains/web/subdomains)

**Issues:**

- **Order of Sections:** The `Quickstart` guide appears after the `Tools & Libraries` section, which can confuse new developers who are unsure where to begin.

- **Inconsistent Inclusion of Code Examples:** While many pages provide helpful code examples, some crucial sections like `Subdomains` lack them.

**Suggestions:**

- **Reorder Sections:** Place the `Quickstart` guide before the `Tools & Libraries` section to guide new developers through the setup process before introducing additional tools.

- **Ensure Consistency in Code Examples:** Review all developer-focused pages to include relevant code examples, ensuring that newcomers have practical references.

---

### 4. Address Technical Issues with the Documentation Site

**Issue:**

- **Site Rendering Problems:** The documentation site sometimes stops rendering content after being inactive, requiring a manual refresh. This disrupts the workflow of developers who switch between coding and reading docs.
  <img width="393" alt="Screenshot 2024-09-22 at 5 53 37 AM" src="https://github.com/user-attachments/assets/40ab007f-7f4c-44e8-b48d-1fa3fdc0e2a3">

**Suggestion:**

- **Fix Rendering Bugs:** Investigate and resolve the rerendering issues to ensure the site remains responsive, even after periods of inactivity.

---

**Conclusion:**

Improving these aspects of the ENS documentation will significantly enhance the developer experience, particularly for those new to blockchain and ENS. Clear, consistent, and comprehensive documentation empowers developers to integrate ENS smoothly into their projects.

I'm more than willing to contribute further by providing code examples or assisting with documentation updates. Please let me know how I can help.

Thank you for your consideration.

---

**Additional Notes:**

- **Mentoring Experience:** My involvement with new developers during the hackathon highlighted these pain points, emphasizing the need for clearer guidance in the documentation.

- **Offer to Contribute:** If the team is open to it, I can submit pull requests with the suggested changes and code examples to help expedite the improvements.

Please feel free to reach out if any clarification is needed on the feedback provided.

Best regards,
Eason
