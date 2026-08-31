\# TLS \& Certificate Security Audit



\*\*Author:\*\* Dennis Kalinin  

\*\*Date:\*\* August 31, 2026  

\*\*Assessment Type:\*\* Passive TLS and Certificate Configuration Review  

\*\*Tools Used:\*\* Qualys SSL Labs, OpenSSL  



\---



\## Executive Summary



This assessment reviewed the TLS and certificate configurations of three public-facing websites to compare intentionally weak configurations with a modern production deployment.



The assessment identified deprecated TLS protocol support, legacy cipher suites, and an incomplete certificate chain across the two badssl.com test environments. GitHub was included as a production comparison and demonstrated a significantly stronger TLS configuration, including TLS 1.3, a complete certificate chain, HTTP Strict Transport Security (HSTS), and modern cipher support.



\### Key Findings



| Target | SSL Labs Grade | Primary Finding | Severity |

|---|---:|---|---|

| `tls-v1-1.badssl.com` | B | Deprecated TLS 1.0 and TLS 1.1 enabled | Medium |

| `incomplete-chain.badssl.com` | B | Incomplete certificate chain and deprecated TLS support | Medium |

| `github.com` | A+ | Strong modern TLS configuration | Low |



\---



\## Scope and Methodology



The assessment was limited to passive TLS and certificate inspection. No exploitation, brute-force testing, credential attacks, or intrusive vulnerability scanning was performed.



Each target was evaluated using two methods:



\### Qualys SSL Labs



SSL Labs was used to review:



\- Overall TLS security grade

\- Certificate validity and trust

\- Certificate-chain configuration

\- Supported TLS protocol versions

\- Supported cipher suites

\- Additional TLS security controls



\### OpenSSL



OpenSSL was used to independently validate:



\- Certificate-chain verification

\- Negotiated TLS protocol versions

\- Negotiated cipher suites

\- Deprecated TLS protocol support

\- Certificate trust errors



\---



\# Finding 1 — Deprecated TLS Protocol Support



\## Target



`tls-v1-1.badssl.com`



\### Assessment Summary



| Item | Result |

|---|---|

| SSL Labs Grade | B |

| Certificate Status | Valid and trusted |

| Certificate Chain | Complete |

| TLS 1.3 | Not supported |

| TLS 1.2 | Enabled |

| TLS 1.1 | Enabled |

| TLS 1.0 | Enabled |

| SSL 3 | Disabled |

| SSL 2 | Disabled |

| Severity | \*\*Medium\*\* |



\### Description



SSL Labs assigned the server an overall grade of \*\*B\*\*. The certificate was valid and trusted, with no certificate-chain issues identified.



The primary weakness was continued support for deprecated TLS versions. Both TLS 1.0 and TLS 1.1 were enabled, which caused SSL Labs to cap the server's grade at B.



The server also offered multiple legacy cipher suites, including CBC-based suites, static RSA suites, and 3DES.



\### Risk



A compatible client could negotiate deprecated TLS versions and weaker cryptographic protections. This increases exposure to downgrade scenarios and attacks that target weaknesses found in older TLS protocols and cipher suites.



While modern clients may prefer stronger TLS 1.2 configurations, continuing to support outdated protocols unnecessarily increases the server's attack surface.



\### Recommended Remediation



Disable TLS 1.0 and TLS 1.1 at the web server, reverse proxy, or load balancer.



Remove legacy cipher suites where compatibility requirements allow, including:



\- 3DES

\- Legacy CBC-based cipher suites

\- Static RSA key-exchange suites



The preferred configuration should use:



\- TLS 1.2

\- TLS 1.3

\- AES-GCM

\- ChaCha20-Poly1305

\- Forward-secret ECDHE key exchange



\### Command-Line Evidence



A normal OpenSSL connection successfully negotiated TLS 1.2:



```text

Protocol: TLSv1.2

Cipher: ECDHE-RSA-AES128-GCM-SHA256

Verification: OK

Verify return code: 0 (ok)

```



TLS 1.1 was explicitly requested and successfully negotiated:



```text

Protocol  : TLSv1.1

Cipher    : ECDHE-RSA-AES128-SHA

Verify return code: 0 (ok)

```



TLS 1.0 was also explicitly requested and successfully negotiated:



```text

Protocol  : TLSv1

Cipher    : ECDHE-RSA-AES128-SHA

Verify return code: 0 (ok)

```



These tests independently confirmed the deprecated protocol support reported by SSL Labs.



\---



\# Finding 2 — Incomplete Certificate Chain



\## Target



`incomplete-chain.badssl.com`



\### Assessment Summary



| Item | Result |

|---|---|

| SSL Labs Grade | B |

| Leaf Certificate | Valid |

| Certificate Chain | \*\*Incomplete\*\* |

| Certificates Provided | 1 |

| TLS 1.3 | Not supported |

| TLS 1.2 | Enabled |

| TLS 1.1 | Enabled |

| TLS 1.0 | Enabled |

| Severity | \*\*Medium\*\* |



\### Description



SSL Labs reported that the server's certificate chain was incomplete and capped the overall grade at \*\*B\*\*.



Although the server certificate itself was within its validity period, the server supplied only the leaf certificate for `\*.badssl.com`. The required intermediate certificate was not sent to the client.



The server also retained support for TLS 1.0 and TLS 1.1 and offered multiple legacy cipher suites.



\### Risk



An incomplete certificate chain does not directly allow an attacker to decrypt encrypted traffic. However, clients that cannot independently retrieve the missing intermediate certificate may fail to establish trust or may display certificate warnings.



Repeated certificate warnings can also encourage unsafe user behavior. Users who become accustomed to bypassing certificate errors may be more likely to ignore a warning associated with an actual impersonation or man-in-the-middle attempt.



\### Recommended Remediation



Configure the web server to send the complete certificate chain in the proper order:



1\. Server or leaf certificate

2\. Required intermediate CA certificate or certificates



Where supported, use the certificate authority's provided full-chain certificate bundle.



The server should also:



\- Disable TLS 1.0

\- Disable TLS 1.1

\- Remove unnecessary legacy cipher suites

\- Enable TLS 1.3 where supported



\### Command-Line Evidence



OpenSSL established a TLS connection but reported certificate validation errors:



```text

verify error:num=20:unable to get local issuer certificate

verify error:num=21:unable to verify the first certificate

```



The final verification result was:



```text

Verify return code: 21 (unable to verify the first certificate)

```



The certificate chain displayed by OpenSSL contained only the server certificate:



```text

Certificate chain

&#x20;0 s:CN=\*.badssl.com

&#x20;  i:C=US, O=Let's Encrypt, CN=YR2

```



The negotiated TLS connection itself used:



```text

Protocol: TLSv1.2

Cipher: ECDHE-RSA-AES128-GCM-SHA256

```



This confirms that the primary certificate issue was the missing intermediate certificate rather than a failure to establish an encrypted connection.



\---



\# Finding 3 — Modern Production TLS Configuration



\## Target



`github.com`



\### Assessment Summary



| Item | Result |

|---|---|

| SSL Labs Grade | \*\*A+\*\* |

| Certificate Status | Valid and trusted |

| Certificate Chain | Complete |

| TLS 1.3 | Enabled |

| TLS 1.2 | Enabled |

| TLS 1.1 | Disabled |

| TLS 1.0 | Disabled |

| SSL 3 | Disabled |

| SSL 2 | Disabled |

| HSTS | Enabled |

| DNS CAA | Present |

| Severity | \*\*Low\*\* |



\### Description



GitHub received an overall \*\*A+\*\* rating from SSL Labs.



The server supported TLS 1.2 and TLS 1.3 while disabling TLS 1.0, TLS 1.1, SSL 2, and SSL 3.



The certificate was valid and trusted, and SSL Labs reported no certificate-chain issues.



GitHub also implemented additional TLS and certificate protections, including:



\- HTTP Strict Transport Security (HSTS)

\- DNS Certification Authority Authorization (CAA)

\- Forward secrecy

\- TLS downgrade protection

\- Modern TLS 1.3 cipher suites



SSL Labs identified some older TLS 1.2 CBC and static RSA cipher suites as weak compatibility options. However, modern clients negotiated significantly stronger configurations.



\### Risk



No significant weakness was observed in the default connection path during this assessment.



Modern clients are able to negotiate TLS 1.3 with authenticated encryption and a trusted certificate chain.



Some legacy TLS 1.2 cipher suites remain available for compatibility. These suites provide weaker protections than GitHub's preferred modern configurations and could be reduced further as older-client compatibility requirements decrease.



\### Recommended Remediation



Continue reducing legacy TLS 1.2 compatibility cipher suites where operational requirements allow.



Maintain the existing use of:



\- TLS 1.2 and TLS 1.3

\- Modern AEAD cipher suites

\- Forward secrecy

\- HSTS

\- DNS CAA

\- Complete certificate chains

\- Certificate lifecycle monitoring



\### Command-Line Evidence



OpenSSL successfully established a trusted TLS 1.3 connection:



```text

Verification: OK

Protocol: TLSv1.3

Cipher: TLS\_AES\_128\_GCM\_SHA256

Verify return code: 0 (ok)

```



The temporary key exchange used X25519:



```text

Peer Temp Key: X25519, 253 bits

```



The server also supplied a certificate chain that OpenSSL successfully validated.



\---



\## Comparative Analysis



The assessment demonstrated a clear difference between simply enabling HTTPS and maintaining a mature TLS security configuration.



The two badssl.com test environments supported deprecated TLS 1.0 and TLS 1.1 protocols and offered numerous legacy cipher suites. In contrast, GitHub disabled these deprecated protocols and supported only TLS 1.2 and TLS 1.3.



The incomplete-chain test also demonstrated that certificate validity alone is not enough to guarantee a trusted HTTPS connection. Although the leaf certificate itself was valid, failing to provide the required intermediate CA certificate caused OpenSSL certificate validation to fail.



GitHub demonstrated a stronger production configuration through a combination of modern protocols, complete certificate chains, HSTS, DNS CAA, forward secrecy, and modern TLS 1.3 cipher suites.



The OpenSSL testing independently supported the SSL Labs findings. The intentionally weak server accepted both TLS 1.0 and TLS 1.1 when explicitly requested, while GitHub negotiated TLS 1.3 with AES-GCM during the default connection.



\---



\## Overall Assessment



| Target | Severity | Assessment |

|---|---|---|

| `tls-v1-1.badssl.com` | Medium | Deprecated protocols and legacy cipher support |

| `incomplete-chain.badssl.com` | Medium | Incomplete certificate chain and legacy TLS configuration |

| `github.com` | Low | Strong production TLS configuration |



\### Conclusion



This assessment demonstrates that secure TLS deployment requires more than simply installing a valid certificate.



Organizations must maintain the full certificate trust chain, remove deprecated protocols, retire weak cipher suites, and regularly review TLS configurations as cryptographic standards evolve.



The strongest configuration reviewed was GitHub's production environment, which combined a valid certificate chain with TLS 1.3, HSTS, DNS CAA, and modern cipher support. The two intentionally vulnerable badssl.com environments showed how deprecated protocols and certificate-chain misconfiguration can weaken otherwise encrypted connections.



\*\*Overall Risk Rating: Medium\*\*

