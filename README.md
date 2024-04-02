# LLM-Hallucination

https://mp.weixin.qq.com/s/qAYImdyACrhd4ipMNh39XA

研究者们分析认为，这种现象可能是多模态大模型在生成较长语句时展现的一种“自动总结”本能。这些“柱状”特征所对应的 token 正是模型推理过程中的 summary token.
由于现有多模态大模型的基座取自大语言模型，其因果语言模型的特点使其在浅层时将前文 token 的信息聚合到 summary token，同时在深层时主要利用 summary token 中聚合的信息来预测整个序列的下一个 token（见下图图 a）。
对此，研究者们给出了基于信息流的解释：他们认为在生成的文本序列越来越长的同时，通常位于序列前段的 vision tokens 所提供的视觉信息会在 summary token 之间信息流动的过程中逐渐被稀释（因为一个 summary token 很难将序列中所有前文 token 所包含的信息都完整地记录）。
因此，越往后生成的 token 越容易忽视 vision tokens，并“过度信赖”某些 summary tokens，从而产生幻觉内容。研究者们将这一现象描述为 “partial over-trust”，并发现大模型的这种阶段性总结可能是导致幻觉问题的一大“元凶”！同时，研究者们进行了数值统计，在不同模型中都观察到了这一现象与幻觉之间的相关性。

研究者们希望通过改变解码策略来缓解这种“过度信赖”现象的出现，从而来减轻幻觉问题。在经典解码方法 Beam Search 的基础上，他们首先在解码过程中对每个 token 的输出概率引入了一个额外的惩罚项，来惩罚其出现“过度信赖”的注意力特征。


由于这种“过度信赖”的特征具有“滞后性”，即只有在解码的过程中输出了若干 token 之后才能发现这样的特征。为了解决这种滞后带来的局限性，研究者们还提出了“回退-再分配”的策略。

