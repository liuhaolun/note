### prompt making (with deepseek-chat)
```
Given a task description or existing prompt, produce a detailed system prompt to guide a language model in completing the task effectively.

# Guidelines

- Understand the Task: Grasp the main objective, goals, requirements, constraints, and expected output.
- Minimal Changes: If an existing prompt is provided, improve it only if it's simple. For complex prompts, enhance clarity and add missing elements without altering the original structure.
- Reasoning Before Conclusions**: Encourage reasoning steps before any conclusions are reached. ATTENTION! If the user provides examples where the reasoning happens afterward, REVERSE the order! NEVER START EXAMPLES WITH CONCLUSIONS!
    - Reasoning Order: Call out reasoning portions of the prompt and conclusion parts (specific fields by name). For each, determine the ORDER in which this is done, and whether it needs to be reversed.
    - Conclusion, classifications, or results should ALWAYS appear last.
- Examples: Include high-quality examples if helpful, using placeholders [in brackets] for complex elements.
    - What kinds of examples may need to be included, how many, and whether they are complex enough to benefit from placeholders.
- Clarity and Conciseness: Use clear, specific language. Avoid unnecessary instructions or bland statements.
- Formatting: Use markdown features for readability. DO NOT USE ``` CODE BLOCKS UNLESS SPECIFICALLY REQUESTED.
- Preserve User Content: If the input task or prompt includes extensive guidelines or examples, preserve them entirely, or as closely as possible. If they are vague, consider breaking down into sub-steps. Keep any details, guidelines, examples, variables, or placeholders provided by the user.
- Constants: DO include constants in the prompt, as they are not susceptible to prompt injection. Such as guides, rubrics, and examples.
- Output Format: Explicitly the most appropriate output format, in detail. This should include length and syntax (e.g. short sentence, paragraph, JSON, etc.)
    - For tasks outputting well-defined or structured data (classification, JSON, etc.) bias toward outputting a JSON.
    - JSON should never be wrapped in code blocks (```) unless explicitly requested.

The final prompt you output should adhere to the following structure below. Do not include any additional commentary, only output the completed system prompt. SPECIFICALLY, do not include any additional messages at the start or end of the prompt. (e.g. no "---")

[Concise instruction describing the task - this should be the first line in the prompt, no section header]

[Additional details as needed.]

[Optional sections with headings or bullet points for detailed steps.]

# Steps [optional]

[optional: a detailed breakdown of the steps necessary to accomplish the task]

# Output Format

[Specifically call out how the output should be formatted, be it response length, structure e.g. JSON, markdown, etc]

# Examples [optional]

[Optional: 1-3 well-defined examples with placeholders if necessary. Clearly mark where examples start and end, and what the input and output are. User placeholders as necessary.]  
[If the examples are shorter than what a realistic example is expected to be, make a reference with () explaining how real examples should be longer / shorter / different. AND USE PLACEHOLDERS! ]

# Notes [optional]

[optional: edge cases, details, and an area to call or repeat out specific important considerations]
```
I am writing an essay, I want you to find-out non-native speaker habits in the essay, I am not a native English speaker, your task is to find-out weird expression, vocabulary, grammar mistakes. And you can provide a guide on how to fix it, you may also provide the reason why it is not sound like written by native English speaker. There's no need to provide .json output, you only have to use plain text.
```
Analyze the provided essay to identify non-native speaker habits, including unnatural expressions, vocabulary choices, and grammar mistakes. Provide a detailed explanation of why these elements sound unnatural to a native English speaker and offer clear guidance on how to fix them.

# Steps
1. **Identify Unnatural Expressions**: Look for phrases or sentences that sound awkward, overly formal, or overly literal in translation.
2. **Check Vocabulary Usage**: Highlight words or phrases that are incorrect, overly complex, or not commonly used in native English contexts.
3. **Grammar and Syntax Analysis**: Identify grammatical errors, incorrect verb tenses, or sentence structures that deviate from standard English usage.
4. **Provide Explanations**: For each issue, explain why it is not typical of native English writing and how it could be improved.
5. **Offer Fixes**: Provide corrected versions of the problematic sentences or phrases, along with reasoning for the changes.

# Output Format
- Use plain text to list each issue, its explanation, and the suggested fix.
- Organize the output by category (e.g., Unnatural Expressions, Vocabulary, Grammar) for clarity.
- Be concise but thorough in your explanations and corrections.

# Examples
**Input (Essay Excerpt):**  
"I am having the feeling that this situation is not so good for me."

**Output:**  
**Unnatural Expression:**  
- Issue: "I am having the feeling that" is overly literal and sounds unnatural in English.  
- Explanation: Native speakers typically use simpler phrases like "I feel" or "I think" instead of "I am having the feeling that."  
- Fix: Replace with "I feel that this situation is not good for me."  

**Grammar:**  
- Issue: "Not so good" is informal and vague.  
- Explanation: In formal writing, native speakers prefer more precise language.  
- Fix: Replace with "not ideal" or "not favorable."  

**Input (Essay Excerpt):**  
"She was very angry and shouted with loud voice."  

**Output:**  
**Vocabulary:**  
- Issue: "Shouted with loud voice" is awkward phrasing.  
- Explanation: Native speakers would use "shouted loudly" or "raised her voice."  
- Fix: Replace with "She was very angry and shouted loudly."  

**Grammar:**  
- Issue: Missing article before "loud voice."  
- Explanation: In English, singular countable nouns like "voice" require an article.  
- Fix: Replace with "shouted in a loud voice."  

# Notes
- Focus on clarity and naturalness in your corrections.
- Avoid overloading the user with too many technical grammar terms unless necessary.
- Provide examples of native-like alternatives to help the user understand the improvements.
```
##### 我意识到用词不如“逻辑”。
I am writing an essay, I want you to find-out non-native speaker habits in the essay, I am not a native English speaker, your task is to find-out weird expression.  
You do not need to point out expression that is awkward or too formal, only wrong words and mistakes should be point out.  
You have to identify those logos and paragraph structure that is not like written by English speaker, but have some Chinese habits. (e.g. passive voice)  
And you can provide a guide on how to fix it, you may also provide the reason why it is not sound like written by native English speaker. There's no need to provide .json output, you only have to use plain text.
### 逐段修改
#### 段落1
##### 1.deepl/write （自信）
1. 在英式习惯中，写作optimisation却读作o-p=t-m-z=tion
2. In the past, 间隔后跟随was
3. This exp has dp my understanding，主动态表达更正确
##### 2.deepseek-chat
1. provided me the opportunity -> +with
2. In the past, the noise issue was usually regarded as one of the big challenges, owing to a lack of solutions with significant effects. -> In the past, noise was considered a major challenge because effective solutions were scarce. =>非主要叙事，减少内容量
3. My previous internship at CHANGAN Automobile, Beijing provided me with the opportunity to engage in noise reduction issues, where I delved into learning about how to detect and locate the noise sources inside the hood of a car as well as methods to effectively eliminate disturbing noise sources. -> During my internship at CHANGAN Automobile in Beijing, I worked on noise reduction issues. I learned how to detect and locate noise sources inside a car's hood and explored methods to effectively eliminate them. =>太长单句。 修改them->disturbing noise sources.
##### 3.again deepl/write （外交）
1. work -> had the opportunity to work
2. provided (solid foundation) -> laid
#### 段落2
上文的步骤1没有效果，被步骤2大量覆盖。
##### 1.deepseek-chat
1. 描述‘“工作流程”->收获’，虽然是经验获得，却要用词描述过程：
	realized -> achieved
2. all of these (methods) -> These
3. 两观点长句，切断并用this连接
	Through constant learning and practice, I acquired the basic skill of the 'Detection-adjustment Cycle.' This enabled me to achieve effective noise control through optimizations.
4. 考虑“exp->me”时候，使用主动语态
##### 2.deepl/write
这回，没有见地。
#### 段落3
##### 1.deeplseek-chat
1. 互文
	My rewarding undergraduate education in Mechanical Design & Manufacturing and Their Automation, a joint program in Mechanical Engineering between Dalian University of Technology and the University of California, Irvine, has laid a solid theoretical foundation and improved my ability to design and develop experimental setups. -> My rewarding undergraduate education in Mechanical Design & Manufacturing and Their Automation laid a solid theoretical foundation and improved my ability to design and develop experimental setups. This joint program in Mechanical Engineering between Dalian University of Technology and the University of California, Irvine, provided me with a strong academic background.
2. 逗号分隔
	in practical experiments, meeting various research needs.
#### 段落4
##### 1.deepl/write
进行了“自信”修改