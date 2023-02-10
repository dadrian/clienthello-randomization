**TL:DR**: The browser will randomize the order it encodes extensions in the TLS ClientHello message. This reduces risk of ossification by external implementers that makes it difficult to deploy future modifications to TLS.

**Background**: The TLS protocol used for HTTPS begins with a ClientHello message. This message contains a set of extensions.

**Problem**: Using a fixed extension order can encourage server implementers to fingerprint specific browsers and then assume specific implementation behavior. This can limit ecosystem agility when browsers implement future modifications to TLS, if the server implementations are not prepared for browsers to change their ClientHello.

**Solution**: The TLS RFC, Section 4.2, specifies that extensions MAY appear in any order.

> When multiple extensions of different types are present, the extensions MAY appear in any order, with the exception of "pre_shared_key" (Section 4.2.11) which MUST be the last extension in the ClientHello

The browser will randomly order extensions, subject to the `pre_shared_key` constraint in the RFC. This will reduce the risk of server and middleboxes fixating on details of current ClientHello orderings. This should make the TLS ecosystem more robust to changes.

**Risks**: It is possible that browser extension ordering is already ossified. This change may cause compatibility issues with middleboxes. We will do a slow rollout and monitor breakage.

**Goals**: This is not designed to prevent all fingerprint of TLS stacks. It is to ensure the ecosystem: 
- is prepared for the structure of the ClientHello to change over time
- does not hardcode byte-offsets / assume specific orderings of fields
