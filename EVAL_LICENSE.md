# Alethia Evaluation License

**Effective: this version, 2026**
**Licensor: vitron.ai (the "Licensor")**
**Software: the Alethia desktop runtime and the Alethia headless runtime, in binary form, including any documentation, configuration files, or sample data distributed with them (collectively, the "Software")**

This Evaluation License Agreement (the "Agreement") is a legal agreement between you, either as an individual or on behalf of a legal entity ("You" or "Licensee"), and vitron.ai, governing Your installation and use of the Software. By downloading, installing, executing, or accepting an in-product prompt to install the Software, You agree to be bound by the terms of this Agreement. If You do not agree to these terms, do not download, install, or use the Software.

---

## 1. Definitions

1.1 **"Software"** means the binary distribution of the Alethia desktop runtime and/or the Alethia headless runtime made available by Licensor, including all updates, bug fixes, and accompanying files.

1.2 **"Evaluation Use"** means installation and execution of the Software for internal, non-commercial testing of applications that You own or control, where: (a) the Software navigates only to `localhost`, `127.0.0.1`, `file://`, `.local`, or private-network (RFC 1918) origins; (b) the Software is not used to generate or export evidence packs, WCAG accessibility audits, or NIST security audits for reliance by any third party, including an auditor, regulator, or customer; (c) no more than one individual uses a given installation at a time; and (d) the Software is not used to provide services to, or on behalf of, any third party. For the avoidance of doubt, routine execution of the Software's core test-authoring and test-execution tools against Your own application under development — including within Your own continuous integration or continuous deployment pipeline — is Evaluation Use and does not by itself constitute Production Use, provided the conditions of this Section 1.2 are otherwise satisfied.

1.3 **"Production Use"** means any use of the Software that does not qualify as Evaluation Use under Section 1.2, including without limitation any use (a) in or in connection with a live, customer-facing, or revenue-generating system or service; (b) to provide services to any third party, whether or not for compensation; (c) generating or exporting evidence packs, WCAG audits, or NIST audits for reliance by any third party; (d) by more than one individual against a single installation; (e) navigating to any origin other than those permitted under Section 1.2(a); (f) in any system handling personal data, financial data, health information, or other regulated data; or (g) in any system for which downtime, defect, or behavioral change of the Software would have material business impact. Production Use requires a separate written commercial license from Licensor and is **not** permitted under this Agreement.

1.4 **"Patent"** means United States Patent Application Number 19/571,437, the corresponding provisional application Number 63/785,814, any continuations, divisionals, reissues, or reexaminations thereof, any patents issuing from any of the foregoing, and any other patents or patent applications now or hereafter owned or controlled by Licensor that read on the Software or the methods practiced by the Software.

1.5 **"Reverse Engineering"** means decompilation, disassembly, decryption, extraction of source code, structural analysis of binary instructions, dynamic instrumentation for the purpose of recovering algorithms, or any other technique whose purpose is to derive the source code, internal architecture, algorithms, data structures, or trade secrets embodied in the Software.

---

## 2. License Grant

2.1 Subject to Your continuous compliance with this Agreement, Licensor grants You a personal, limited, non-exclusive, non-transferable, non-sublicensable, revocable license to:

(a) install one or more copies of the Software on computing devices that You own or control; and

(b) execute the Software on those devices solely for Evaluation Use;

for so long as this Agreement remains in effect, as described in Section 6.

2.2 The license granted in Section 2.1 is the entirety of the rights granted to You under this Agreement. **All rights not expressly granted are reserved by Licensor.**

---

## 3. NO PATENT LICENSE — READ THIS SECTION CAREFULLY

3.1 **No license, express or implied, by estoppel, exhaustion, or otherwise, is granted to You under any Patent by this Agreement, by Your installation or use of the Software, by any prior or contemporaneous oral or written statement by Licensor, or by any conduct of Licensor.** The right to install and execute the Software for Evaluation Use granted under Section 2 does **not** include and shall **not** be construed to include any right under any Patent.

3.2 The Software embodies methods, systems, and apparatus claimed in the Patent. Licensor's distribution of the Software to You under this Agreement is an exercise of Licensor's exclusive rights under the Patent, not a transfer or sharing of those rights. You receive no right under the Patent to make, have made, use (other than the limited Evaluation Use authorized by Section 2), sell, offer for sale, import, distribute, sublicense, or otherwise practice the inventions claimed in the Patent, whether by means of the Software or by any other means.

3.3 Without limiting the foregoing, You acknowledge and agree that:

(a) the inventions claimed in the Patent are independent of the Software and are not transferred or licensed to You by virtue of Your possession of the Software;

(b) any production use of the inventions claimed in the Patent — including any reimplementation, variation, derivative, or independent re-creation of the techniques observed during Your Evaluation Use — would, absent a separate written patent license from Licensor, infringe the Patent and expose You to claims for damages and injunctive relief;

(c) the foregoing applies regardless of whether You at any time obtain a separate license under this Agreement, a commercial license, or any other agreement permitting use of the Software in a different scope, unless that subsequent agreement expressly grants You rights under the Patent in writing.

3.4 **A separate, written commercial license from Licensor is required for any use of the inventions claimed in the Patent beyond the Evaluation Use authorized by Section 2.** Inquiries: gatekeeper@vitron.ai.

---

## 4. Restrictions

You shall not, and shall not permit any third party to:

(a) use the Software for any Production Use;

(b) copy, reproduce, modify, adapt, translate, or create derivative works of the Software, except for the limited internal copies necessary for Evaluation Use;

(c) distribute, sell, lease, rent, sublicense, lend, transfer, assign, host, time-share, or otherwise make the Software available to any third party, in whole or in part;

(d) perform Reverse Engineering of the Software, including without limitation any decompilation, disassembly, instrumentation for source recovery, or any attempt to derive the algorithms, internal data structures, or trade secrets embodied in the Software, except to the minimum extent that this restriction is expressly prohibited by applicable law and only after providing Licensor with thirty (30) days' prior written notice and a reasonable opportunity to provide the necessary information in source form;

(e) remove, alter, obscure, or replace any proprietary notices, copyright notices, patent notices, license terms, version markings, build identifiers, integrity hashes, or trademark legends contained in or displayed by the Software;

(f) use the Software to develop, train, evaluate, or improve any product, service, or technology that competes with the Software, including without limitation any AI-agent-driven test runtime, browser automation system, deterministic execution engine, or per-step policy enforcement system;

(g) publish, disclose, or otherwise make available to any third party any benchmark, performance comparison, security analysis, vulnerability report, or other technical assessment of the Software, in whole or in part, without the prior written consent of Licensor;

(h) circumvent, disable, or interfere with any license enforcement, feature-gating, integrity verification, or local audit-logging mechanism contained in the Software;

(i) use the Software in violation of any applicable law, regulation, or third-party right; or

(j) export, re-export, or transfer the Software in violation of any United States export control law or regulation.

---

## 5. Intellectual Property

5.1 The Software, the Patent, all copyrights in the Software, all trademarks associated with the Software (including without limitation "Alethia," "VITRON-EA1," and "vitron.ai"), and all other intellectual property rights in and to the Software, are and shall remain the exclusive property of Licensor. This Agreement does not transfer to You any ownership interest in the Software or in any such intellectual property rights.

5.2 Any feedback, suggestions, bug reports, feature requests, or other information You voluntarily provide to Licensor regarding the Software ("Feedback") may be used by Licensor for any purpose without compensation, attribution, or restriction, and You hereby grant Licensor a perpetual, irrevocable, worldwide, royalty-free, sublicensable license to use, copy, modify, distribute, and create derivative works of the Feedback for any purpose. You shall not provide Feedback that is subject to any obligation of confidentiality to a third party.

---

## 6. Term and Termination

6.1 **Term.** The license granted under Section 2 begins on Your first installation of the Software and continues, for Evaluation Use only, without a fixed expiration date, unless earlier terminated under this Section 6.

6.2 **Change of Terms.** Licensor may update the scope of Evaluation Use described in Section 1.2 on a prospective basis by posting a revised version of this Agreement or a successor agreement, effective as to future use. Such an update does not retroactively affect Your compliance for use prior to the update. Requests regarding scope not covered by Section 1.2: gatekeeper@vitron.ai.

6.3 **Termination for Breach.** This Agreement and all rights granted hereunder terminate automatically and without notice upon any breach by You of any term of this Agreement.

6.4 **Termination for Convenience.** Licensor may terminate this Agreement and revoke all rights granted hereunder at any time, for any reason or no reason, by providing notice through the Software, by email, or by posting a notice on the Licensor's website.

6.5 **Effect of Termination.** Upon termination or expiration of this Agreement for any reason, You shall (a) immediately cease all use of the Software, (b) delete all copies of the Software in Your possession or under Your control, and (c) certify in writing to Licensor that You have done so if requested. The provisions of Sections 3 (No Patent License), 4 (Restrictions), 5 (Intellectual Property), 7 (Disclaimer), 8 (Limitation of Liability), 9 (Governing Law), and 10 (General) survive any termination or expiration of this Agreement.

---

## 7. DISCLAIMER OF WARRANTIES

THE SOFTWARE IS PROVIDED **"AS IS"** AND **"AS AVAILABLE"**, WITH ALL FAULTS AND WITHOUT WARRANTY OF ANY KIND. TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, LICENSOR DISCLAIMS ALL WARRANTIES, WHETHER EXPRESS, IMPLIED, STATUTORY, OR OTHERWISE, INCLUDING WITHOUT LIMITATION ANY IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE, NON-INFRINGEMENT, ACCURACY, RELIABILITY, AVAILABILITY, AND ANY WARRANTY ARISING OUT OF COURSE OF DEALING OR USAGE OF TRADE. LICENSOR DOES NOT WARRANT THAT THE SOFTWARE WILL MEET YOUR REQUIREMENTS, THAT THE OPERATION OF THE SOFTWARE WILL BE UNINTERRUPTED OR ERROR-FREE, THAT DEFECTS WILL BE CORRECTED, OR THAT THE SOFTWARE OR ANY SERVERS THAT MAKE THE SOFTWARE AVAILABLE ARE FREE OF VIRUSES OR OTHER HARMFUL COMPONENTS.

THE SOFTWARE IS DISTRIBUTED FOR EVALUATION USE ONLY AND IS NOT INTENDED OR LICENSED FOR USE IN ANY ENVIRONMENT WHERE FAILURE OR INCORRECT BEHAVIOR COULD CAUSE DEATH, PERSONAL INJURY, PROPERTY DAMAGE, OR FINANCIAL LOSS.

---

## 8. LIMITATION OF LIABILITY

TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, IN NO EVENT SHALL LICENSOR BE LIABLE TO YOU OR TO ANY THIRD PARTY FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, EXEMPLARY, OR PUNITIVE DAMAGES, INCLUDING WITHOUT LIMITATION DAMAGES FOR LOST PROFITS, LOST REVENUE, LOST DATA, BUSINESS INTERRUPTION, COST OF SUBSTITUTE GOODS OR SERVICES, OR ANY OTHER PECUNIARY OR NON-PECUNIARY LOSS, ARISING OUT OF OR RELATED TO THIS AGREEMENT OR THE SOFTWARE, WHETHER IN CONTRACT, TORT (INCLUDING NEGLIGENCE), STRICT LIABILITY, OR ANY OTHER THEORY, EVEN IF LICENSOR HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.

LICENSOR'S TOTAL CUMULATIVE LIABILITY TO YOU UNDER OR IN CONNECTION WITH THIS AGREEMENT, FROM ALL CAUSES OF ACTION AND ALL THEORIES OF LIABILITY, SHALL NOT EXCEED **THE GREATER OF (A) ONE HUNDRED UNITED STATES DOLLARS ($100.00) OR (B) THE TOTAL AMOUNT YOU HAVE PAID TO LICENSOR FOR THE SOFTWARE UNDER THIS AGREEMENT** (which, for the avoidance of doubt, is zero, because the Software is provided to You free of charge under this evaluation license).

THE PARTIES ACKNOWLEDGE THAT THE LIMITATIONS IN THIS SECTION ARE A FUNDAMENTAL BASIS OF THE BARGAIN AND THAT LICENSOR WOULD NOT MAKE THE SOFTWARE AVAILABLE TO YOU ON THE TERMS OF THIS AGREEMENT IN THE ABSENCE OF SUCH LIMITATIONS.

---

## 9. Governing Law and Jurisdiction

9.1 This Agreement and any dispute arising out of or in connection with this Agreement or the Software shall be governed by and construed in accordance with the laws of the State of Delaware, United States of America, without regard to its conflict-of-laws principles. The United Nations Convention on Contracts for the International Sale of Goods does not apply to this Agreement.

9.2 You consent to the exclusive personal jurisdiction and venue of the state and federal courts located in the State of Delaware for any action arising out of or relating to this Agreement, and You waive any objection to such jurisdiction and venue.

9.3 Notwithstanding the foregoing, Licensor may seek injunctive or equitable relief in any court of competent jurisdiction to protect its intellectual property rights, including without limitation its rights under the Patent.

---

## 10. General

10.1 **Entire Agreement.** This Agreement constitutes the entire agreement between You and Licensor regarding the Software and supersedes all prior or contemporaneous communications, proposals, and representations regarding the same subject matter.

10.2 **Severability.** If any provision of this Agreement is held to be unenforceable, that provision shall be modified to the minimum extent necessary to make it enforceable, and the remaining provisions shall remain in full force and effect.

10.3 **No Waiver.** Failure by Licensor to enforce any provision of this Agreement shall not be deemed a waiver of that or any other provision.

10.4 **Assignment.** You may not assign or transfer this Agreement, in whole or in part, without Licensor's prior written consent. Licensor may freely assign this Agreement.

10.5 **Export Controls.** You acknowledge that the Software may be subject to United States export control laws and regulations. You agree to comply with all such laws and regulations and not to export or re-export the Software in violation of them.

10.6 **U.S. Government Users.** The Software is "commercial computer software" as defined in 48 C.F.R. § 2.101. If acquired by or on behalf of a unit or agency of the United States Government, all rights in the Software are governed by the terms of this Agreement.

10.7 **Notices.** All notices to Licensor under this Agreement shall be sent to: **gatekeeper@vitron.ai**.

---

## 11. Acceptance

By installing or executing the Software, You acknowledge that You have read this Agreement, understand it, and agree to be bound by its terms. If You do not agree, do not install or execute the Software, and delete all copies in Your possession.

---

**Patent Notice.** The Software embodies methods claimed in United States Patent Application Number 19/571,437 (non-provisional), claiming priority to United States Provisional Application Number 63/785,814 (filed April 9, 2025), titled *"Deterministic Local Automation Runtime with Zero-IPC Execution, Offline Operation, and Per-Step Policy Enforcement."* Patent pending — United States Patent and Trademark Office. Licensing inquiries beyond the scope of this evaluation: **gatekeeper@vitron.ai**.

Copyright © 2025–2026 vitron.ai. All rights reserved.
