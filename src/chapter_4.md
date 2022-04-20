### 4. Zusammenfassung

Bei der Betrachtung des de Casteljau Algorythmus und dessen Anwendung in der Praxis, wie in [3.2](./chapter_3/IneffizienzUndSchwierigkeitenInDerPraxis.md) ist aufgefallen, dass der Algorythmus gerade für das Zeichnen solcher Bezierkurven aufgrund der Rekursivität, sowie Abhängigkeit von \\(t\\) in einem pixelbasierten rendering System ungeeignet ist.

Durch genauere Betrachtung der mathematishen Definition einer solchen Bezierkurve und das Nutzen der Mitternachtsformel konnte ich die folgende Formel wie in [3.3](./chapter_3/MathematischerLoesungsansatz.md) herleiten, die nun punktspezifisch angewendet werden kann.

$$ t_{1,2} = \frac{-(2P_{1} - 2P_{0}) \pm \sqrt{(2P_{1} - 2P_{0})^2 - 4(P_{0} - 2P_{1} + P_{2})(P_{0} - T)}}{2(P_{0} - 2P_{1} + P_{2})} $$

Weiterführend zu dieser Arbeit kann nun die obige Formel genutzt werden, um die praktische Implementierung mithilfe eines Shaders durchzuführen.