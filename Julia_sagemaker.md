
# Getting started with Julia on AWS SageMaker

#aws_whitepaper_summary 

In this article we will explore how to get started combining these powerful tools and start an exciting journey using Julia as an alternative for data science and machine learning.

- This guide will walk you through the following steps:
  - Creating an Amazon SageMaker notebook instance
  - Creating a Julia environment
  - Installing IJulia and running a Julia notebook
  - Testing the Julia notebook

- This guide assumes that you have the following:
  - An AWS account with the capability to create new instances in the Amazon SageMaker console.
  - A basic understanding of machine learning or data processing.


## Setting Up Your Environment

### Creating an Amazon SageMaker Notebook Instance
1. Sign in to Amazon SageMaker console, and open the Amazon SageMaker Dashboard.
2. Select Notebook instances. Alternatively, on the left menu panel, select Notebook instances in the Notebook section.
3. Select Create notebook instance.
4. For Notebook instance name enter the name, and for the Notebook instance type select the instance type for your workload and budget.

![in](https://user-images.githubusercontent.com/23625821/126863890-5b7ff0af-0ed6-43dd-9c5d-3f36d7b91d6e.png)

5. Select Open JupyterLab, to launch a console of your notebook instance

![in](https://user-images.githubusercontent.com/23625821/126863913-57c5d043-b79b-4b58-af91-717a04d7d845.png)

### Creating a Julia Environment
Use the following procedure to create Julia environment in the JupyterLab console.

1. In the JupyterLab console, navigate to the Launcher tab if visible, scroll to the bottom of the tab, and select Terminal.

![in](https://user-images.githubusercontent.com/23625821/126863992-60b529cc-9fe2-4bec-82dc-7bafb1ba8fd0.png)

2. If the Launcher tab is not visible in the JupyterLab console, navigate to the menu bar, and select File, New, Terminal.

![1](https://user-images.githubusercontent.com/23625821/126891474-88fbf608-a884-4375-a55e-ccf807558385.png)

3. A new tab, with a Linux shell console attached, will open.

![1](https://user-images.githubusercontent.com/23625821/126891480-c05fcfb4-edb0-4c79-91ed-ec654e99b96e.png)

4. To create a new julia conda environment for Julia, and to switch into it, run the following command.

```sh
#!/bin/sh
source activate
conda create --yes -n julia
conda activate julia
```
5. To verify your current environment, run the following command.

```sh
#!/bin/sh
conda env list

```

The expected result is shown below. An environment named julia is marked with an asterisk (*) to indicate that it is currently active.

![1](https://user-images.githubusercontent.com/23625821/126891517-22eb11dc-8f8f-4a71-8e00-62597d3c7c57.png)

6. To install the Julia language package and to verify that installation is complete, run the following command.
```sh

#!/bin/sh
conda install --yes -c conda-forge Julia=1.0.3
julia -v

```

7. To verify that Julia is successfully installed, run the following command to launch a Julia REPL console.

```sh
#!/bin/sh

julia

```

You now have successfully set up Julia REPL, and have the full capabilities and features of a Julia runtime environment.

![1](https://user-images.githubusercontent.com/23625821/126891556-d926140a-e844-4adb-b55a-2b8095cddf90.png)


### Installing IJulia and running a Julia Notebook

Use the following procedure to install the Julia kernel (IJulia package) for Jupyter. This will make JupyterLab aware of Julia’s existence, and configure the necessary settings, such as new options for the Julia notebook and kernel.

1. To install IJulia and activate the Julia kernel for JupyterLab, run the following command on the Julia REPL console.

```sh
using Pkg
Pkg.add("IJulia")
using IJulia
jupyterlab(detached=true)

```

2. To launch your first Julia notebook in Amazon SageMaker, on the Launcher tab, choose Julia 1.0.3.

![1](https://user-images.githubusercontent.com/23625821/126891647-f40eff9a-e736-421c-976b-58c90edd70ea.png)

3. To test native Julia code, enter the following snippets into code cells on the newly created Julia notebook.

```julia
versioninfo()
∑(x,y) = x + y
∑(2, 3)
```

![1](https://user-images.githubusercontent.com/23625821/126891670-d2386a23-28ab-400f-83b0-62a497bc8ac4.png)

### Julia Notebook Examples
Use the following procedure to experiment further with how Julia enables you to visualize data.

1. To install the packages Plots and DataFrames, enter the following in to the Julia notebook cells.

```julia

import Pkg
Pkg.add("Plots")
Pkg.add("DataFrames")
```


You will employ Pkg, a built-in package manager, to install Plots and DataFrames, and any required dependent packages will be resolved and installed automatically by Pkg. You should see the following results.

![1](https://user-images.githubusercontent.com/23625821/126891716-b524461a-78d2-4fc6-9f2d-d832fec0a45d.png)


2. To generate a sequence of integers and a sequence of random float values, run the following command.

```julia 
using Random
A = [1:1000...]
B = [randn(Float64) for i in A]
last(A)
```

3. To create a DataFrame object from the two previously created arrays, run the following command.

```julia 
using DataFrames
df = DataFrame(X = A, Y = B)
sort!(df, [:X])
last(df, 5)
```

A DataFrame (df ) object is created, where column X contains the values from array A, and column Y contains the values from array B. Then, object df is sorted by the values in column X, and the last five rows are reported as shown below.

![1](https://user-images.githubusercontent.com/23625821/126891795-dac9cee3-9a25-47cf-9622-7d951dfcb866.png)

4. To plot the data as a scatterplot, run the following command.

```julia 
using Plots

scatter(df.X, df.Y, w=3)
histogram(df.Y, bins=:scott, weights=repeat(1:5, outer=200))

```

### Deleting Your Instance
When your notebook instance is no longer in use, we recommend deleting the instance. Use the following procedure to stop and delete your notebook instance.

1. Navigate to your Notebook instance list, and choose the instance that you want to delete.
2. To stop your instance, select Actions menu, Stop menu. The process of stopping an instance may take several minutes. When it is completed, the status column in your Notebook instance list will show as Stopped.

3. Select the instance that was stopped previously, and select Actions menu, then Delete menu.


#### References 


<a href="https://d1.awsstatic.com/whitepapers/julia-on-sagemaker.pdf?did=wp_card&trk=wp_card"> Original White Paper </a>
