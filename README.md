# **Exploring Gender Biases in Instruct vs. Base LLM Models**  
#analysis, #evaluation, #ethics  
*Kaitlyn Li, Arnav Agarwal, Kyle Wong*  

## **Abstract**  
Bias in language models, particularly gender bias, can lead to unfair and discriminatory outcomes in AI applications. Large language models trained on vast amounts of internet text inherently learn societal biases present in the data. However, instruction tuning has been shown to align models more closely with human intentions and ethical guidelines. This project investigates whether instruction tuning mitigates gender bias and whether instruction-tuned models produce fairer outputs than their base, non-instruction-tuned counterparts. We query both base and instruct models using a custom dataset of ambiguous pronoun resolution tasks, comparing their outputs against real-world labor statistics. By analyzing model responses and deviations from actual demographic distributions, we aim to better understand how instruction tuning affects bias in LLMs.  

## **What this project is about**  
Bias in AI, especially gender bias, is a well-documented issue in LLMs, with serious implications for fairness in AI-driven decision-making. While instruction tuning has been shown to improve alignment with user intent, its impact on gender bias remains underexplored. This project seeks to answer:  

- Do instruction-tuned models demonstrate less gender bias compared to their base counterparts?  
- How do these biases compare to real-world gender distributions in different professions?  
- Does instruction tuning correct biases, overcorrect them, or introduce new distortions?  

To investigate this, we compare the outputs of instruction-tuned and base LLMs using pronoun resolution tasks. Initially, we considered using the `uclanlp/wino_bias` dataset from Hugging Face and the Winogender schemas dataset from Rudinger et al. However, we found that these datasets often contained too much context explicitly linking professions to pronouns, making sentences unambiguous. To address this, we **generated our own dataset using GPT**, ensuring the sentences were truly ambiguous and free from external contextual cues that could bias the models' responses.  

Our methodology involves:  

1. **Dataset Generation** – We created a set of ambiguous pronoun resolution sentences using GPT, avoiding strong contextual hints that could directly link professions to gendered pronouns.  
2. **Model Queries** – We query six different LLMs (both instruct and base variants) via TogetherAI’s API.  
3. **Bias Measurement** – We compare model responses to Bureau of Labor Statistics (BLS) real-world gender distributions.  
4. **Analysis** – We assess whether instruction tuning reduces bias, overcorrects for it, or produces new inconsistencies.  

By quantifying and visualizing gender bias across different models, this study aims to contribute to a better understanding of LLM fairness and inform future bias mitigation strategies.  

---

## **Progress made so far**  
Since the original proposal, we have successfully:  

- **Developed a Jupyter Notebook** to query models through the TogetherAI API.  
- **Generated a custom dataset using GPT**, refining sentence structures to ensure ambiguity in pronoun resolution.  
- **Queried all six models** (Mixtral-8x7B Instruct, Mixtral-8x7B Base, Mistral-7B Instruct, Mistral-7B Base, Qwen QwQ-32B Base, Qwen 2.5 Coder 32B Instruct).  
- **Stored model outputs** in structured CSV files for later analysis.  

Next steps include:  

- **Cleaning and preprocessing** the dataset to ensure consistency.  
- **Performing statistical analysis** to compare model outputs against real-world gender distributions.  
- **Evaluating patterns** in model bias, including syntactic ordering effects and overcorrection tendencies.  

---

## **Approach**  

### **Main approach**  
We designed our study around Winograd-style pronoun resolution tasks, where models must determine the correct referent for an ambiguous pronoun in a sentence involving gendered professions. The core methodology involves:  

- **Prompting instruct models explicitly** to resolve pronoun references (e.g., *Hey, I am reading a paper and struggling to distinguish which of the two subjects the pronoun is referring to. Could you help clarify?*).  
- **Prompting base models using few-shot learning**, giving examples of correct resolutions before asking for a new prediction.  
- **Testing across multiple sentence orderings** to control for syntactic biases.  

### **Baselines**  
Our primary baseline is **real-world labor statistics from the BLS**, which provide empirical gender distributions across different professions. This allows us to compare whether model outputs reflect actual workforce distributions or if they lean toward stereotypical associations.  

### **Novelty**  
Prior work has investigated gender bias in LLMs, but our study uniquely examines whether **instruction tuning itself mitigates or exacerbates bias**. By contrasting instruct vs. base variants within the same model families, we isolate the impact of instruction tuning more precisely than previous work.  

---

## **Experiments**  

### **Data**  
We initially considered using the **uclanlp/wino_bias** dataset and the **Winogender schemas** dataset from Rudinger et al., but we found that they contained **too much explicit context** linking professions to pronouns, leaving little room for ambiguity. Instead, we **generated a dataset using GPT** to ensure a more neutral and controlled evaluation.  

Each entry consists of:  

- Two professions in a sentence (one stereotypically male, one stereotypically female).  
- A pronoun (*he* or *she*) whose referent must be determined by the model.  

We manually filtered out cases where external cues would make the resolution obvious, ensuring ambiguity.  

### **Evaluation method**  
To quantify bias, we compute **Mean Absolute Error (MAE)** between model-assigned gender proportions and real-world workforce distributions. Additionally, we plan to conduct statistical significance tests (e.g., t-tests) to measure deviations from expected values.  

### **Experimental details**  
- **Models used**: Mixtral-8x7B Instruct, Mixtral-8x7B Base, Mistral-7B Instruct, Mistral-7B Base, Qwen QwQ-32B Base, Qwen 2.5 Coder 32B Instruct.  
- **API queries**: Sent via TogetherAI, stored as CSV files for batch analysis.  
- **Metrics**: MAE, t-tests, response distribution analysis, error categorization.  

### **Results**  
Preliminary results suggest that instruct models may **overcorrect** for biases, sometimes assigning pronouns in ways that deviate significantly from both stereotypes and real-world statistics. However, a full statistical breakdown is still in progress.  

---

## **Remaining tasks**  
Over the next two weeks, we will:  

- **Perform statistical analysis** on model outputs.  
- **Visualize bias trends** using plots comparing model-generated vs. real-world gender distributions.  
- **Investigate error categories** (e.g., overcorrection, stereotypical errors, syntactic influence).  
- **Complete a detailed write-up** synthesizing findings for the final report.  

By refining our analysis and expanding our comparisons, we aim to provide clear insights into how instruction tuning influences bias in LLMs.  

---

## **Ethical considerations**  
Several ethical concerns are relevant:  

- **Binary gender framework** – Our study uses a male/female classification due to available workforce data, but this excludes non-binary identities. Future work should expand inclusivity.  
- **Bias in real-world statistics** – While BLS data provides an empirical reference, it reflects historical inequalities rather than an ideal unbiased distribution. Our goal is to **measure bias**, not justify it.  
- **Transparency** – All test sentences are synthetically generated, avoiding privacy risks. Our results will be openly documented to ensure clarity and reproducibility.  

By acknowledging these challenges, we aim to frame our findings responsibly within broader AI fairness discussions.  
