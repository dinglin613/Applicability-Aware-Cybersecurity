# Notation

This page summarizes the notation used in the paper. Symbols are grouped by the
part of the framework in which they appear.

## Topology and Indices

| Symbol | Meaning |
| --- | --- |
| $\mathcal{T}=(\mathcal{D},\mathcal{F},\mathcal{E})$ | Cyber-physical IEC 61850 topology. |
| $\mathcal{D}$ | Set of cyber-relevant devices. |
| $N=\lvert\mathcal{D}\rvert$ | Number of cyber-relevant devices. |
| $d_i$ | Device indexed by $i$. |
| $\mathcal{F}$ | Set of operational functions. |
| $M=\lvert\mathcal{F}\rvert$ | Number of operational functions; used in complexity expressions. |
| $f$ | Operational-function index. |
| $f_1,\ldots,f_4$ | SCADA Observability, Substation Control, Protection Integrity, and Automation Continuity. |
| $\mathcal{E}$ | MMS client-server and GOOSE publisher-subscriber communication links. |
| $\mathcal{F}_i$ | Set of functions served by device $d_i$. |
| $\mathcal{D}_f$ | Devices serving function $f$: $\{d_i\in\mathcal{D}:f\in\mathcal{F}_i\}$. |
| $\mathcal{Z}$ | Set of protection zones. |
| $z$ | Protection-zone index. |
| $\mathcal{D}_z$ | Devices located in protection zone $z$. |
| $t$ | Discrete maintenance-window epoch. |
| $T$ | Total number of scheduling epochs. |
| $i,j,k$ | Device, CVE, or local CVE-profile indices, depending on context. |

## Device State and Feasibility Constraints

| Symbol | Meaning |
| --- | --- |
| $\mathcal{V}_i=\{v_{i,1},\ldots,v_{i,K_i}\}$ | Candidate CVE profiles associated with device $d_i$. |
| $K_i$ | Number of candidate CVE profiles for device $d_i$. |
| $u_i(t)\in\{0,1\}$ | Patch state of device $d_i$; 0 means unpatched and 1 means patched. |
| $y_i(t)\in\{0,1\}$ | Online state of device $d_i$; 0 means offline during patching and 1 means online. |
| $\rho_i(t)\in[0,1]$ | Network reachability of device $d_i$. |
| $\mathbf{x}_t$ | System state at epoch $t$, containing patch and online states. |
| $\mathbf{a}_t$ | Set of patch actions selected at epoch $t$. |
| $a_{i,t}\in\{0,1\}$ | Binary indicator that device $d_i$ is patched at epoch $t$. |
| $\mathcal{A}_t$ | Feasible action set at epoch $t$. |
| $m_t\in\{0,1\}$ | Maintenance-window indicator; 1 means epoch $t$ is open. |
| $C_{\max}$ | Maximum number of devices that can be patched concurrently. |
| $Z_{\max}$ | Maximum concurrent outages allowed in each protection zone. |
| $N_f^{\min}$ | Minimum number of online devices required for function $f$. |
| $\mathcal{P}$ | Set of precedence-ordered device pairs. |
| $\mathcal{X}$ | Set of mutually exclusive device pairs. |
| $\mathcal{C}$ | Set of cooldown triples. |
| $t_i^{\mathrm{end}}$ | Completion epoch of the most recent patch on device $d_i$. |
| $\delta_{ij}$ | Required cooldown interval between dependent device patches. |
| $\mathcal{G}$ | Complete deterministic feasibility-constraint collection instantiated from C1--C7. |

## Applicability Inference

| Symbol | Meaning |
| --- | --- |
| $c_j$ | Disclosed CVE indexed by $j$. |
| $Q=\{q_{ij}\}$ | Device-CVE applicability matrix. |
| $q_{ij}\in[0,1]$ | Applicability weight of CVE $c_j$ to device $d_i$. |
| $q_{i,k}$ | Local indexing of the applicability weight for CVE profile $v_{i,k}$. |
| $r_{ij}^{(m)}$ | Verdict returned by LLM provider $m$ for pair $(c_j,d_i)$. |
| $q_{ij}^{(m)}$ | Applicability weight mapped from provider $m$'s verdict. |
| $q_u$ | Applicability weight assigned to an uncertain verdict. |
| $\mathcal{M}$ | Set of LLM providers used in the ensemble. |
| $m$ | LLM-provider index. |

## Risk Model

| Symbol | Meaning |
| --- | --- |
| $e_{i,k}\in[0,1]$ | Normalized exploitability indicator for CVE profile $v_{i,k}$. |
| $E_i(Q)$ | Applicability-weighted aggregate exploitability of device $d_i$. |
| $p_i(t;Q)$ | Time-varying compromise proxy for device $d_i$. |
| $\mathbf{1}\{\cdot\}$ | Indicator function. |
| $\varepsilon$ | Non-negligible exploitability threshold used for applicability gates. |
| $\sigma(x)$ | Logistic function, $1/(1+e^{-x})$. |
| $\xi_i(t)\in[0,1]$ | Time-varying exposure signal for device $d_i$. |
| $\alpha,\beta$ | Weights for inherent exploitability and exposure, with $\alpha+\beta=1$. |
| $w_{f,i}\in(0,1]$ | Criticality weight of device $d_i$ for function $f$. |
| $P_f(t;Q)$ | Bounded function-level compromise likelihood score. |
| $C_f(t;Q)$ | Function-level consequence term. |
| $\kappa_f$ | Base criticality of function $f$. |
| $\Psi_f(\mathbf{o}_t)$ | Operating-condition modifier for function $f$. |
| $\mathbf{o}_t$ | Operating-condition vector at epoch $t$. |
| $\mathbf{h}(\cdot)$ | Feature map that normalizes operating conditions to $[0,1]$. |
| $\boldsymbol{\eta}_f$ | Per-function sensitivity coefficient vector. |
| $\psi_{\min},\psi_{\max}$ | Clipping bounds for the operating-condition modifier. |
| $S_f(t;Q)$ | Function-level severity index. |
| $\omega_v,\omega_o$ | Weights for unpatched vulnerable fraction and offline fraction, with $\omega_o=1-\omega_v$. |
| $R_t(Q)$ | System-level risk index at epoch $t$. |
| $R_{\mathrm{base}}$ | Common pre-patching risk used as the CER baseline. |
| $\gamma_i$ | Shorthand $w_{f,i}p_i(t;Q)$ in the marginal-risk expression. |
| $\Delta P_f(j\mid\mathcal{U})$ | Reduction in $P_f$ from patching $d_j$ given already-patched set $\mathcal{U}$. |

## Scheduling Objective and Search

| Symbol | Meaning |
| --- | --- |
| $\pi:\mathbf{x}_t\mapsto\mathbf{a}_t$ | Scheduling policy mapping state to patch actions. |
| $J(\pi)$ | Finite-horizon objective combining risk and operational burden. |
| $D_t$ | Normalized operational burden at epoch $t$. |
| $\tau_i$ | Offline duration required to patch device $d_i$. |
| $T_{\mathrm{ref}}$ | Reference duration used to normalize patching burden. |
| $\lambda$ | Risk-burden trade-off weight. |
| $H$ | Receding-horizon lookahead depth. |
| $B$ | Candidate cap for branch expansion. |
| $F$ | Frontier cap in the bounded lookahead search; distinct from the function set $\mathcal{F}$. |
| $W=\min(t+H,T)$ | End of the current lookahead window. |
| $L=T-W$ | Remaining tail length after the lookahead window. |
| $n$ | Search-node index. |
| $\mathcal{U}^{(n)}$ | Patched-device set stored at search node $n$. |
| $\mathcal{D}_{\mathrm{cand}}^{(n)}(Q)$ | Applicability-gated candidate set at search node $n$. |
| $\Delta R_i^{(n)}(Q)$ | One-step risk reduction from patching $d_i$ at node $n$. |
| $\phi_i^{(n)}$ | Marginal risk-to-burden score used for candidate ranking. |
| $\hat{J}_{\mathrm{tail}}$ | Tail-cost estimate at a leaf node. |
| $\mathcal{U}(\tau)$ | Completed-patch set at tail epoch $\tau$. |
| $\mathcal{I}_f$ | Devices in $\mathcal{D}_f$ patched within the tail continuation. |
| $P_f^0,S_f^0$ | Function likelihood and severity at the beginning of the tail bound. |
| $s_j$ | Start epoch of a tail patch on device $j$ in Proposition 2. |

## Evaluation Metrics and Experiment Labels

| Symbol | Meaning |
| --- | --- |
| CER | Cumulative Exposure Reduction, $\sum_t[R_{\mathrm{base}}-R_t]$. |
| ELS | Exposure-weighted load-shed metric in MWh. |
| $\mathcal{V}_Q(t)$ | Applicability-gated set of unpatched vulnerable devices at epoch $t$. |
| $\mu_d$ | Per-device exposure weight derived from the corresponding CVE score. |
| $s_d$ | Load-shed value associated with the N-1 outage mapped to device $d$. |
| HN-FPR | Hard-negative false-positive rate in the applicability benchmark. |
| $\kappa$ | Cohen's kappa for annotator agreement. |
| B0 | Unconstrained CVSS-ordering baseline. |
| B1 | Constraint-enforced severity/CVSS-ordering baseline. |
| B2 | Deadline-driven baseline. |
| B3 | NIST-inspired rule-ordering baseline. |
| B4 | Successive-linearization MILP baseline. |
| B5 | Random feasible scheduling baseline. |
| S1 | Baseline open-window operating scenario. |
| S2 | Peak-load restricted-window operating scenario. |
| S3 | Peak-load restricted-window scenario with spoofing exposure signal. |
