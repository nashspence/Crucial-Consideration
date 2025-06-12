## Minimal Formalism

Below is a concise setup capturing the key novel aspects: **(a)** utilities that evolve as a function of incoming signals, and **(b)** a “self-defeat” metric for actions whose future continuation might be judged negative by a later utility.

---

### 1. Histories and Beliefs

- **Histories:**  
  \[
    H_{t} \;=\; \bigl\{\,(a_0,z_1,a_1,z_2,\dots,a_{t-1},\,z_{t})\bigr\}, 
    \quad H_0 = \varnothing.
  \]
  A particular history is denoted \(h_t \in H_t\).

- **Belief Update:**  
  Let \(\mathcal{S}\) be the (hidden) state space and \(\mathcal{Z}\) the signal space.  The agent holds a belief  
  \[
    b_t \;=\; \Beta\bigl(b_{\,t-1},\,z_t\bigr)
    \;\in\; \Delta(\mathcal{S}),
    \qquad b_0 \text{ given.}
  \]

---

### 2. Evolving Utility (Preferences)

- **Utility Space:**  
  \(\mathcal{U}\) is the set of all utility‐functionals  
  \[
    U: \bigl((a_t,z_{t+1},a_{t+1},\dots,a_{T-1},z_T)\bigr)
    \;\longrightarrow\; \mathbb{R}.
  \]
- **Preference‐Update Rule:**  
  \[
    U_t \;=\; \Phi\bigl(h_t\bigr),
    \qquad
    \Phi:\; \bigl(H_t\bigr)\;\longrightarrow\;\mathcal{U}.
  \]
  Equivalently, if one writes \(U_t\) recursively via \(\Psi\), then
  \[
    U_t \;=\; \Psi\bigl(U_{t-1},\,z_t\bigr),
    \quad
    U_0 \text{ given.}
  \]
  In particular, each new signal \(z_t\) can cause a **transformative jump** in the utility‐functional.

---

### 3. Policy and Value Recursion

- **Actions:**  
  At time \(t\), choose \(a_t\in\mathcal{A}\).  A (history‐dependent) policy is  
  \[
    \pi_t:\; H_t \;\longrightarrow\; \Delta(\mathcal{A}).
  \]
- **One‐Step Payoff:**  
  Given \(h_t,a_t,z_{t+1}\), the current utility \(U_t\) yields 
  \[
    r_t\bigl(h_t,a_t,z_{t+1}\bigr)\;\in\;\mathbb{R}.
  \]
- **Bellman‐Type Recursion with Evolving Utility:**  
  Define
  \[
    V_T(h_T) \;=\; U_T(\emptyset)
    \;=\; \Phi(h_T)\bigl(\emptyset\bigr).
  \]
  For \(t < T\),
  \[
    V_t(h_t)
    \;=\; 
    \max_{\,a_t\in\mathcal{A}} 
    \;\mathbb{E}_{\,z_{t+1}\,\sim\,W(\cdot\mid h_t,a_t)}\Bigl[\,
      r_t\bigl(h_t,a_t,z_{t+1}\bigr)
      \;+\;\gamma\,V_{t+1}\bigl(h_{t+1}\bigr)
    \Bigr],
  \]
  where 
  \[
    h_{t+1} \;=\; \bigl(h_t,\,a_t,\,z_{t+1}\bigr)
    \qquad\Longrightarrow\qquad
    U_{t+1} \;=\; \Phi(h_{t+1}).
  \]
  Thus at each node \(h_t\), the **continuation value** \(V_{t+1}(h_{t+1})\) is evaluated under the newly updated \(U_{t+1}\).

---

### 4. Self-Defeat Metric

To quantify how likely choosing \(a_t\) at \(t\) will be judged *negative* by a future utility \(U_{t'}\), define:

1. **Future Utility Trajectory Distribution:**  
   From \((h_t,\,a_t)\), signals \(\{z_{t+1},\dots,z_T\}\) are drawn via the world model \(W\).  These induce utilities  
   \[
     U_{t+1} = \Psi(U_t,\,z_{t+1}),\; 
     U_{t+2} = \Psi(U_{t+1},\,z_{t+2}),\;\dots
   \]
   yielding a joint distribution  
   \(\mu\bigl(\{z_{t+1:t'}\},\{U_{t+1:t'}\}\mid h_t,\,a_t\bigr)\).

2. **“Self-Defeat” Event:**  
   For each future \(t'>t\), given a realized continuation path \(\mathbf{x}_{t':T}=(a_{t'},z_{t'+1},\dots)\), the value under \(U_{t'}\) is  
   \[
     U_{t'}(\mathbf{x}_{t':T}).
   \]
   A *self-defeat alarm* at time \(t'\) occurs if
   \[
     U_{t'}\bigl(\mathbf{x}_{t':T}\bigr)\;<\;0.
   \]
3. **Discounted Self-Defeat Number:**  
   \[
     \mathrm{SD}(a_t \mid h_t)
     \;=\;
     \sum_{\,t' = t+1}^{T}\; 
     \gamma^{\,t' - t} \;
     \Pr_{\mu}\Bigl(\,
       U_{t'}\bigl(\mathbf{x}_{t':T}\bigr) < 0 
       \;\Bigm|\; 
       h_t,\,a_t
     \Bigr).
   \]
   If one cares only about *whether* any future regret occurs, define the Boolean version
   \[
     \mathrm{SD}^\star(a_t \mid h_t)
     \;=\;
     \Pr_{\mu}\Bigl(\exists\,t'>t:\; U_{t'}(\mathbf{x}_{t':T}) < 0
       \;\Bigm|\; h_t,\,a_t
     \Bigr).
   \]

4. **Action Selection with Self-Defeat Penalty:**  
   Introduce a penalty weight \(\lambda \ge 0\).  At \(h_t\), pick
   \[
     a_t^\star 
     \;=\; 
     \arg\max_{\,a\in\mathcal{A}}
     \Bigl\{
       \underbrace{\mathbb{E}_{\,z_{t+1}}\Bigl[r_t(h_t,a,z_{t+1}) \;+\;\gamma\,V_{t+1}(h_{t+1})\Bigr]}_{\text{“value under }U_t\text{”}}
       \;-\;\lambda\,\mathrm{SD}(a \mid h_t)
     \Bigr\}.
   \]

---

### 5. Key Novel Aspects Emphasized

1. **History‐Dependent Utility:**  
   \[
     U_t \;=\; \Phi(h_t), 
     \quad
     \Phi:\; H_t \to \mathcal{U},
   \]
   allows **transformative jumps** in preferences whenever new signals arrive.

2. **Dynamic Inconsistency:**  
   Because \(U_{t+1}\neq U_t\) in general, the agent’s ex-ante plan under \(U_t\) may differ from the action she actually wants under \(U_{t+1}\).  Our recursion “re-solves” at each node with the updated utility.

3. **Self-Defeat Metric:**  
   \[
     \mathrm{SD}(a_t \mid h_t) 
     \;=\; 
     \sum_{t'>t}\;\gamma^{\,t'-t}\;\Pr\bigl(U_{t'}(\mathbf{x}_{t':T})<0 \mid h_t,a_t\bigr),
   \]
   quantifies the chance that a given action’s continuation is later judged **net‐negative** by a shifted utility.  This directly captures the core intuition of “self-defeat” in a world of evolving preferences.

---

> **Notes:**  
> - One may replace the finite horizon \(T\) with \(T=\infty\) if \(\gamma<1\).  
> - In practice, \(\mathrm{SD}(a_t \mid h_t)\) can be approximated by Monte‐Carlo sampling of future \(\{z_{t+1},\dots\}\) and tracking induced \(U_{t'}\).  
> - If \(\lambda\) is large, the agent avoids actions with high self-defeat potential even if they look appealing under \(U_t\).  
