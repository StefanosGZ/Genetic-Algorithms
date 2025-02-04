<h1>First Tutorial: Genetic Algorithm (GA) - Read Me</h1>
<p>This notebook demonstrates how to implement a Genetic Algorithm (GA) using Python. The solution involves optimizing a simple test function called the Sphere function. Below, you will find a guide to understanding and running the code provided in the notebook.</p>
<h2>Table of Contents</h2>
<ol>
<li><a title="" target="_blank" href="#introduction">Introduction</a></li>
<li><a title="" target="_blank" href="#installation">Installation</a></li>
<li><a title="" target="_blank" href="#problem-definition">Problem Definition</a></li>
<li><a title="" target="_blank" href="#ga-parameters">GA Parameters</a></li>
<li><a title="" target="_blank" href="#core-functions">Core Functions</a></li>
<li><a title="" target="_blank" href="#ga-execution">GA Execution</a></li>
<li><a title="" target="_blank" href="#results">Results</a></li>
<li><a title="" target="_blank" href="#conclusion">Conclusion</a></li>
</ol>
<h2>Introduction</h2>
<p>This tutorial covers the implementation of a basic GA for optimizing the Sphere Test Function, which is a common test case in optimization:</p>
<p><span class="katex-display"><span class="katex"><span class="katex-mathml"><math display="block" xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>f</mi><mo stretchy="false">(</mo><mi>x</mi><mo stretchy="false">)</mo><mo>=</mo><mo>∑</mo><msubsup><mi>x</mi><mi>i</mi><mn>2</mn></msubsup></mrow>f(x) = \sum{x_i^2}</math></span><span aria-hidden="true" class="katex-html"><span class="base"><span style="height:1em;vertical-align:-0.25em;" class="strut"></span><span style="margin-right:0.10764em;" class="mord mathnormal">f</span><span class="mopen">(</span><span class="mord mathnormal">x</span><span class="mclose">)</span><span style="margin-right:0.2778em;" class="mspace"></span><span class="mrel">=</span><span style="margin-right:0.2778em;" class="mspace"></span></span><span class="base"><span style="height:1.6em;vertical-align:-0.55em;" class="strut"></span><span style="position:relative;top:0em;" class="mop op-symbol large-op">∑</span><span style="margin-right:0.1667em;" class="mspace"></span><span class="mord"><span class="mord"><span class="mord mathnormal">x</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span style="height:0.8641em;" class="vlist"><span style="top:-2.453em;margin-left:0em;margin-right:0.05em;"><span style="height:2.7em;" class="pstrut"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathnormal mtight">i</span></span></span><span style="top:-3.113em;margin-right:0.05em;"><span style="height:2.7em;" class="pstrut"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight">2</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span style="height:0.247em;" class="vlist"><span></span></span></span></span></span></span></span></span></span></span></span></p>
<p>Where <span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>x</mi></mrow>x</math></span><span aria-hidden="true" class="katex-html"><span class="base"><span style="height:0.4306em;" class="strut"></span><span class="mord mathnormal">x</span></span></span></span> is a vector of variables.</p>
<h2>Installation</h2>
<p>To run this notebook, you will need Python and some associated packages. It's recommended to use Google Colab for an easy setup. Run the following command to install the required package:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">!pip install ypstruct
</code></pre>
</div>
</div>
<p>Make sure <code>ypstruct</code> is installed, as it's a helper package used to define the problem and parameters structure in a clean and efficient way.</p>
<h2>Problem Definition</h2>
<p>Define the optimization problem using the <code>structure</code> from <code>ypstruct</code>:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">from ypstruct import structure

# Sphere Test Function
def sphere(x):
    return sum(x**2)

# Problem definition
problem = structure()
problem.costfunc = sphere
problem.nvar = 5
problem.varmin = [-10, -10, -1, -5, 4]
problem.varmax = [10, 10, 1, 5, 10]
</code></pre>
</div>
</div>
<h2>GA Parameters</h2>
<p>Set up the GA parameters and their respective values:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">params = structure()
params.maxit = 100
params.npop = 50
params.pc = 1
params.gamma = 0.1
params.mutation_rate = 0.01
params.sigma = 0.1
params.beta = 1
</code></pre>
</div>
</div>
<h2>Core Functions</h2>
<h3>Crossover Function</h3>
<p>Performs crossover operation between two parents to generate offspring:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">def crossover(p1, p2, gamma=0.1):
    c1 = p1.deepcopy()
    c2 = p2.deepcopy()
    alpha = np.random.uniform(-gamma, 1 + gamma, *c1.position.shape)
    c1.position = alpha * p1.position + (1 - alpha) * p2.position
    c2.position = alpha * p2.position + (1 - alpha) * p1.position
    return c1, c2
</code></pre>
</div>
</div>
<h3>Mutation Function</h3>
<p>Applies mutation to an individual:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">def mutate(x, mutation_rate, sigma):
    y = x.deepcopy()
    flag = np.random.rand(*y.position.shape) &lt;= mutation_rate
    ind = np.argwhere(flag)
    y.position[ind] += sigma * np.random.randn(*ind.shape)
    return y
</code></pre>
</div>
</div>
<h3>Apply Bounds Function</h3>
<p>Ensures the variables are within defined minimum and maximum bounds:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">def apply_bounds(x, varmin, varmax):
    x.position = np.maximum(x.position, varmin)
    x.position = np.minimum(x.position, varmax)
    return x
</code></pre>
</div>
</div>
<h3>Roulette Wheel Selection Function</h3>
<p>Selects individuals based on their probability derived from fitness:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">def roulette_wheel_selection(probabilities):
    cumulative_sum = np.cumsum(probabilities)
    r = sum(probabilities) * np.random.rand()
    ind = np.argwhere(r &lt;= cumulative_sum)
    return ind[0][0]
</code></pre>
</div>
</div>
<h3>GA Execution Function</h3>
<p>Runs the entire GA process:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">def run(problem, params):
    # function body here
</code></pre>
</div>
</div>
<h2>GA Execution</h2>
<p>Run the Genetic Algorithm with the defined problem and parameters:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">out = run(problem, params)
</code></pre>
</div>
</div>
<h2>Results</h2>
<p>The results are displayed showing the best cost found during each iteration. The plot helps visualize the optimization process:</p>
<div class="code-block-parent">
<button class="copy-code-button"><i aria-label="Copy code the below code snippet" class="pi pi-copy"></i> Copy code</button>
<div class="code-block-container">
<pre style="overflow-x: auto;"><code class="language-python">plt.plot(out.bestcost)
plt.xlim(0, params.maxit)
plt.xlabel('Iterations')
plt.ylabel('Best Costs')
plt.title('Genetic Algorithm (GA)')
plt.grid(True)
plt.show()
</code></pre>
</div>
</div>
<h2>Conclusion</h2>
<p>This notebook demonstrates a basic Genetic Algorithm for solving an optimization problem. By using this template, you can extend the algorithm to other optimization problems with different cost functions and parameters.</p>
<p>Feel free to experiment with different configurations to understand the impact on the optimization results. Happy coding!</p>
