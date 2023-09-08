# Advanced Computing Systems 2023-2024 Lab 1 - Matrix Multiplication

This repository contains all files and explanations needed to perform
the first lab of the CESE4010 - Advanced Computing Systems course at the
Delft University of Technology.

## What do I have to do for Lab 1?

Quick version:

* Download the template project.
* Implement the matrix multiplication functions in the template project, 
  using the following techniques:
  * SIMD extensions (AVX)
  * Multi-core (OpenMP)
  * GPU Accelerator (OpenCL)
* Implement the experiments that benchmark these functions.
* Make sure all tests pass.
* Benchmark your solution.
* Optimize your implementations.
* Benchmark your optimizations.
* Write a report.
* Turn in your report.
* Rejoice with the knowledge that you have gained.

The long version:

The questions below can be read as a sequence, and they will become a
sort of lab manual!

## What preliminary skills should I have to complete this lab?

* You need to be proficient in C/C++.
* You need to be able to use the GNU/Linux command-line (bash).
* You need to be able to use LaTeX to write a report.

These are not only skills to help you in this course, but they are used in
many master courses. If you miss any of them, it is recommended to learn
them as soon as possible.

### Help! I cannot C/C++!
If you have never written applications using C/C++, it is highly likely that
you will find this course too difficult to complete. You might want to take 
some bachelor courses in programming C/C++ before attempting this course.
You should at least be able to work with pointers and dynamic allocation
functions such as `malloc` and `free`.

### Help! I cannot GNU/Linux command line (bash)!
While there are numerous tutorials on how to use the GNU shell (bash), it might
be helpful to keep 
[a cheat-sheet](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/bash.sh)
at hand.

### Help! I cannot LaTeX!
Very nice open-source books about LaTeX
[exist](https://en.wikibooks.org/wiki/LaTeX).

### Help! I do not have a GNU/Linux distribution on my laptop/computer.
It's a very good idea to get your computer to dual boot a GNU/Linux distribution.
[Ubuntu](https://www.ubuntu.com/download/desktop)
is a good distribution for beginners with a large userbase that have probably
asked and answered all questions you can think of already somewhere online.

#### I am extremely stubborn and I do not want to use GNU/Linux!

[MobaXTerm](https://mobaxterm.mobatek.net/) is a pretty good terminal client
for Windows.

## Can I fork this repository as a student?

Answer: __do not fork it publicly__

#### Why not? This is ridiculous!

According to examination rules and regulations, if you made it possible to 
commit fraud, you are partially responsible. Therefore, if you fork this 
repository publicly and you commit changes to it, and other students plagiarize
from your repository, you are partially responsible for the fraud.

We didn't make those rules, but let's respect them to not get into trouble!

## What is AVX?
AVX stands for Advanced Vector Extensions. They are additional instructions
to the x86 ISA to perform operations on vectors. For example, they make it
possible to not just add two single-precision floating point numbers with one
instruction, but multiple single-precision floating point numbers with one
instruction, at the same time. This can speed up all sorts of algorithms 
executed on a CPU. These extensions are commonly found in microprocessors used
in laptops to workstations to HPC clusters. It is important to know that they 
are available and how to use them.

## What is OpenMP?
OpenMP stands for Open Multi-Processing. It is a set of compiler directives and
libraries to make it easy to write multi-threaded programs. Because nowadays
many microprocessors have multiple CPU cores in them, it is important to know
how to use OpenMP and that it can help you to quickly parallelize parts of your
program.

## What is OpenCL?
OpenCL stands for Open Compute Language. This is a standard that is meant to 
provide the means of writing portable programs for heterogeneous systems, such
as computers that have a GPU or FPGA. A similar proprietary framework called
CUDA exists. CUDA is very similar to OpenCL, although it runs well mainly on 
NVIDIA GPUs. While OpenCL enjoys less matured tooling for NVIDIA GPUs,
more matured tools are available for other types of platforms, such as FPGAs.

## Where to start?
1. You should accept this assignment through GitHub Classroom. Typically, the link can be located on Brightspace, following this format: https://classroom.github.com/x/xxxxxxxx.
2. Sign in to GitHub using your personal GitHub account.
3. On the subsequent page, you and your team members need to create your groups using the format: "group-xx", where "xx" represents the Brightspace group number. For example: "group-01" and "group-50". If your group members (or TAs) already create it, then just join. If you joined a wrong group, or created a wrong team, contact TA.
4. Accept the assignment on the subsequent page.
5. Now you should see a page telling you __"You accepted the assignment, 2023-lab1 . We're configuring your repository now. This may take a few minutes to complete. Refresh this page to see updates."__
6. Refresh the page and click the repository link (https://github.com/tud-acs/2023-lab1-group-xx). If you encounter any issues during this step, reach out to the TA for assistance.
7. When you see your repository, your journey begins.

## How can I compile the code on my local computer?
1. Set up your ssh key pairs for GitHub. You can generate a ssh key pair with `ssh-keygen` on Ubuntu. After generation, upload your public key to your [GitHub setting](https://github.com/settings/keys). The public key is located at `~/.ssh/id_rsa.pub`, you can print it in terminal via `cat ~/.ssh/id_rsa.pub`
2. Clone your repository to your computer (use ssh protocol, e.g. `git@github.com:tud-acs/2023-lab1-groupxx.git`).
3. Install C++ complier and cmake: `sudo apt install gcc g++ cmake`.
4. Install OpenCL. There are numerous implementations of OpenCL. Pick one that corresponds to your available hardware.
* [Intel](https://software.intel.com/en-us/articles/opencl-drivers)
* [AMD](https://www.amd.com/en-us/solutions/professional/hpc/opencl)
* [NVIDIA](https://developer.nvidia.com/opencl)
* For Ubuntu, you can quickly get OpenCL with `sudo apt install ocl-icd-opencl-dev`
5. Find the template code in the `app` directory: `cd {2023-lab1-group-xx}/app`.
6. Compile the template project. A CMake script has been supplied with the template code.
```console
mkdir debug
cd debug
cmake ..
```

These commands will create a build directory called `debug`, go into that directory, and will let CMake 
create the project files there. Note that the CMake script will exclude files
from the build where you are supposed to implement technologies that your 
systems doesnt have any support for (it will complain about this on the 
command line).

7. Now you can build the project with: `make`. And run it with: `./acsmatmult -h` to show help information.

## The [GitHub CI](https://docs.github.com/en/actions/automating-builds-and-tests/about-continuous-integration) (continuous integration)

We use GitHub CI to test your code automatically. When you push a new commit to the `development` branch, a runner will try to build & run your code. The CI traces are available on your GitHub repository page. Notice that the CI traces will be removed after one day, so record your results in time.

<div>
<img src="https://raw.githubusercontent.com/tud-acs/lab1-ci/development/tutorial/imgs/github_ci.png" height="800">
</div>

Be aware that the performance results from the `development` runners might not be accurate as multiple runners share hardware resources. You need to push your code to a new branch called `benchmark` to use the `benchmark` runners who have dedicated CPU and GPU.

<div>
<img src="https://raw.githubusercontent.com/tud-acs/lab1-ci/development/tutorial/imgs/github_ci_benchmark_branch.jpg" height="800">
</div>

If the `benchmark` branch already exists, you need to create a pull request to push the changes in `development` branch to `benchmark` branch.

<div>
<img src="https://raw.githubusercontent.com/tud-acs/lab1-ci/development/tutorial/imgs/github_ci_pull_request.png" height="800">
</div>

You are allowed to modify the [CI file](https://github.com/tud-acs/lab1-ci/blob/development/.github/workflows/cmake.yml) to customize your run arguments. For instance, you can alter line 38 as shown below to measure CPU time:
```
run: time ./acsmatmult -a
```

You are not allowed to change critical CI configurations, such as line 20: 
```
timeout-minutes: 5
```

You can check the Runner status at "Your GitHub repository page" -> Settings -> Actions -> Runners. If you crashed a runner or find a runner is down, contact TA.

## Help! I don't have a GPU / CPU with AVX support / multicore processor.

You can test your code on:
1. [GitHub Runner](https://github.com/organizations/tud-acs/settings/actions/runners) by commiting your code to the `development` branch. 
2. [Google Colab](https://colab.research.google.com/) provides free GPU resources (with daliy usage limit) and you can upload the this [Jupyter Notebook](https://github.com/tud-acs/lab1-ci/blob/development/Lab1.ipynb) to Colab and your source code to Google Drive to build & run. Please be aware that you must enable GPU to use OpenCL.

<div>
<img src="https://raw.githubusercontent.com/tud-acs/lab1-ci/development/tutorial/imgs/colab.jpg" height="600">
</div>


<!-- Although possible, you do not necessarily have to debug all parts of your 
application on these machines. It is recommended to first functionally test and
debug your application on a local machine before benchmarking on these nodes. -->

<!-- Most modern laptops, for example, have vector extensions with multiple cores,
so you should at least be able to implement the vector / AVX / SIMD functions 
and the OpenMP functions. You might not have a GPU in your laptop, but still 
most modern Intel and AMD CPUs can be used to run OpenCL kernels as well. -->

<!-- ## How do I connect to the GPU cluster?

Log in to the `student-linux.tudelft.nl`:

```console
ssh <netid>@student-linux.tudelft.nl
```
From there, log in to __one__ of the login nodes:

```console
ssh login1.hpc.tudelft.nl
ssh login3.hpc.tudelft.nl
```

While the course is busy and many people are building, you might want to switch
to one or the other login server. However, remember that the login servers should not be 
used to run benchmarking workloads; you may only use them to test your code with very 
small example data (if your code runs for longer than a couple of seconds on the login server, 
please [kill the process you started](https://www.linuxfoundation.org/blog/classic-sysadmin-how-to-kill-a-process-from-the-command-line/)). 
In order to run benchmarking workloads, submit them to the GPU cluster nodes as discussed in the "[How do I submit a job to one of the GPU cluster nodes?](
#how-do-i-submit-a-job-to-one-of-the-gpu-cluster-nodes)" section below. -->

## What sort of applications am I allowed to run on the Runner?

* You are __only allowed to run jobs that are directly related to the course__
and your studies.

* If you run anything unrelated, such as e.g. cryptocoin miners or video 
  renderers, __you and your group__ will, without warning:
  * immediately fail the course without any chance of resit.
  * be revoked of access to the GitHub Runner.
  
* If you think you do need to run something that might appear suspicious, and 
you are not sure if your application is allowed, it is your responsibility to 
ask first.

## Do I have root access on the Runners / why can't I use sudo?

No, nee, nein, non, 没有, tidak, Không, Όχι, ne, hakuna, yok, 아니, لا, engin, 
Нет, nu, nie, etc..

You will not have root access or superuser rights. If you are reading tutorials
that tell you to "sudo" something, it is __absolutely useless__ to try and
execute that command.

<!-- ## What node am I on right now?

Through multiple / nested SSH sessions it could become confusing which node
you are actually on at the moment. Use this to check out which node you are on:

```console
hostname
``` -->

<!-- ## How to enable GPU support on a node?

GPU support on the nodes is enabled through the use of Environment Modules.
[Environment Modules](http://modules.sourceforge.net/) can be used to quickly
set up your system environment to, for example, change versions of software
packages, etc... -->

<!-- ### Detecting available modules

```console
module use /opt/insy/modulefiles
module avail
```

This will detect the available Environment Modules. It is recommended to use
the latest CUDA version for this lab. -->

<!-- ### Loading the CUDA/OpenCL module
```console
module use /opt/insy/modulefiles
module load cuda/<version>
nvcc --version
``` -->

<!-- ## How do I install CMake?

To build the baseline project, you need a 3.10+ version of CMake. This is available on the HPC cluster as `cmake3`. -->

<!-- ## How do I edit files on the command line?

The default cluster installation comes with `nano` and `vim`. -->

## Is there any good IDE to write C/C++?

If you like IDEs, [CLion](https://www.jetbrains.com/clion/) is by far the best
IDE out there at the moment. You can get it for free as a student, if you sign
up with your TU Delft e-mail account.

## What should I do with the project?

You should read the template source code and figure out how the program works.
The first thing you must benchmark for your report is the template vector
multiplication (also see the [the LaTeX report template](report/template.tex)).

Then, you must:
 
* Implement an experiment for matrix multiplication.

And implement experiments as well after completing each of the following:

* Implement matrix multiplication using SIMD instructions (AVX).
* Implement matrix multiplication on multiple cores using OpenMP.
* Implement matrix multiplication on a GPU using OpenCL.

Optimize your above three implementations (with the knowledge you obtained from the course).

<!-- ## What will the TAs run to test if I've implemented everything correctly?

`acsmatmult -t`

However, be aware that we will only copy the [app/src/acsmatmult/students](app/src/acsmatmult/students)
back into our own baseline project. So anything you change outside that
file will not work for us. Sorry! -->

## Where are the files that I have to implement?

You can find them here:

[app/src/acsmatmult/students](app/src/acsmatmult/students)

## Can I change any other files?

You can do so, but it should not be necessary, unless you find any bugs.
If you find any, please let us know, and we will try to update the template
projects as soon as possible.

Other than that, if you change any of the files, you should explain in your
report why you did so.

## What are good resources to have at hand during the lab?

__HIGHLY RECOMMENDED__ resources are:

* [Intel Intrinsics Guide](https://software.intel.com/sites/landingpage/IntrinsicsGuide/)
    * This link will take you to the Intel intrinsics guide. This is a 
    searchable library of all the x86 intrinsics that an Intel machine might 
    or might not support.
* [OpenMP API C/C++ Syntax Reference Guide](https://www.openmp.org/wp-content/uploads/OpenMP-4.5-1115-CPP-web.pdf)
    * This is a handy cheat sheet. You can also discover a lot of functions
    that OpenMP supplies here. 
* [OpenCL API docs](https://www.khronos.org/registry/OpenCL/sdk/1.2/docs/man/xhtml)
    * We use OpenCL 1.2 in the lab. This links will bring you to the API docs.
    Here you can read the interface specification of various OpenCL functions.
* [OpenCL API 1.2 reference card](https://www.khronos.org/files/opencl-1-2-quick-reference-card.pdf)
    * This is another handy cheat sheet to get a quick overview of all the
    functions that you can use in OpenCL.
    
## Help! Some bug in the template project is preventing me from completing the lab!

While we have tested the deployment of the template project, it is possible
that some bugs exist. Before you report this bug, try to make sure that
it's an actual bug in the template and not in your own code.

Once you have done that, do not panic. The lab instructors are reasonable
people. Let us know as soon as possible. We will try to fix the bug and
if it's something really serious, we may even postpone a deadline (although
you should never count on it).

## How to get CMake to detect OpenCL?

In the CMake build scripts called [CMakeLists.txt](app/CMakeLists.txt), some scripts 
are included that attempt to find OpenCL support. Once you've installed the OpenCL
module, CMake will detect it.

<!-- ## How do I submit a job to one of the GPU cluster nodes?

The cluster uses SLURM to manage the cluster resources and to schedule jobs on
cluster nodes. You can use the `sbatch` command to submit jobs. Usually, these
jobs are bash scripts. 

When you are on the cluster, you can submit jobs using the sbatch command.
A more detailed explanation is recommended reading material, and can be found
[here](https://login.hpc.tudelft.nl/).

You __must__, however, use a specific partition (a group of nodes, let's say) and
a specific quality-of-service (a set of requirements on the resources you can
request from the cluster).

These are:
```console
--account=stud-ewi-crs-cese4010
--partition=cese4010
--qos=cese4010
--reservation=cese4010 
```

When you want to submit a script to SLURM, you must always supply these
arguments to sbatch (or you must put them in a job script, see below).

Example:

```console
sbatch --account=stud-ewi-crs-cese4010 --partition=cese4010 --qos=cese4010 --reservation=cese4010 <job script.sh>
``` -->

<!-- ## Where is the output?

In a file called `slurm-<jobid>.out`. You can easily show the output using, 
for example:

```console
cat slurm-1234.out
``` -->

<!-- ## Do you have a template for a job script?

Allocating both CPU and GPU resurces on node.

```console
#!/bin/sh
#SBATCH --account=stud-ewi-crs-cese4010
#SBATCH --partition=cese4010
#SBATCH --qos=cese4010
#SBATCH --reservation=cese4010
#SBATCH --time=0:01:00
#SBATCH --ntasks=1
#SBATCH --gres=gpu
#SBATCH --cpus-per-task=2
#SBATCH --mem=1024
#SBATCH --mail-type=FAIL

srun <your command>
```

Allocating CPU resurces only on node.

```console
#!/bin/sh
#SBATCH --account=stud-ewi-crs-cese4010
#SBATCH --partition=cese4010
#SBATCH --qos=cese4010
#SBATCH --reservation=cese4010
#SBATCH --time=0:01:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=1024
#SBATCH --mail-type=FAIL

srun <your command>
```

Please consult the HPC cluster presentation for more information about those options. 
If you don't know what you're doing, do not change anything.

Please be aware that if you are going to run the OpenMP benchmark, you might want to
change to `--cpus-per-task=8`. You cannot get more than 8 cores within this specific
QoS. -->

<!-- ## I am editing on my local machine! How do I copy files to the cluster?

Upload the sources to your home directory on `student-linux.tudelft.nl`.
This is in sync with the HPC cluster nodes.

For example, when you are in the lab1 baseline project directory, you can use 
`scp` with the recursive option `-r` to copy the whole `src` directory over at 
once:

```bash
scp -r src <netid>@student-linux.tudelft.nl:~/path/to/your/src
```

Then you can go into a second terminal that is logged in to an HPC cluster
login node, build the sources there and submit a batch job to run it on a 
node with GPU capabilities. -->

## I am getting tired of typing in the commands, this sucks!

It is recommended to automate various steps of this development flow using 
scripts. Any command that you can type on the command-line, you can also put
in a bash script.

* For example, create a new script with:

```console
vim <script name>.sh
```

* Press insert to start typing in vim.
* Press escape and :wq to write the changes to the file and quit vim.
* Make the script executable by running

```console
chmod +x <script name>.sh
```

* Run the script with: 

```console
./<script name>.sh
```

By automating the development flow using scripts, the lab will be less tiring!

## Can I see a list of commands that I typed previously?

Yes, use arrow keys (up and down), or:

```console
history
```

You could even pipe it into grep to search your history, example:

```console
history | grep cmake
```



## Is there a template for the lab report?

Yes. This is [the LaTeX template](report/template.tex) and it is actually
__mandatory__ to use this template. As you can read in the next question, 
any lab reports that do not use this template are denied. This is to make 
sure we can correct your lab reports quickly. In this way, you will receive
your grade and feedback quickly as well.

## When do I measure the performance and put some graphs in the report?

First of all, whenever it is explicitly stated.

Also, after every significant change/improvement. That means when you 
change something and it only gives a 0.1% improvement, you can just write
about what you've changed, but that it didn't change significantly.

However, if you get over, let's say, 5% improvement, then it is worth to
show new graphs. 

Also if you get _less_ performance but did not initially expect so, 
you may write this down. If you show insight on why you did
not get what you've expected, this is seen as a learning opportunity and
thus it is not met with reduced points. On the contrary, we encourage 
this.

## How do I make plots?

It is recommended to use a decent plotting tool (e.g. not Microsoft Excel or LibreOffice Calc). Some good plots can be made with free tools such as matplotlib in Python. You could also use MATLAB or Scilab.

There is an [IPython Notebook](report/plotting_tutorial.ipynb) with an example available here.

Make sure you generate vectorized PDF figures and plots as much as possible, and avoid bit-mapped formats like PNG and JPG. This way, the quality of your figures will increase, while their storage size in Kilo Bytes will decrease.

## What ranges should I measure in terms of problem size / run-time?

The beginning of your measurement range should be as small as possible, to 
identify any initialization overhead.

The end of your measurement range is arrived when measurements take a 
couple of seconds. Then you are in a range where initialization 
overhead of the different techniques becomes somewhat negligible (__for this lab__!). 
However, be aware that you don't have infinite memory.

You also measure everywhere in between. You _could_ make this very fine grained, but
if this doesn't show any anomalies, please refrain from cluttering your plot
and take bigger steps.


## How do I turn in my report?

Only submissions that follow these requirements are accepted:

* __You have used [the LaTeX template](report/template.tex) to create the report.__

* __The file name of your report is GROUP_XX_netid0_netid1_netid2.pdf__
  * Example: GROUP_01_alee_bzhang_cwang.pdf - accepted.
  * Example: GROUP_13_dnguyen_egarcia_fgonzales.pdf - accepted.
  * Example: New Document by n00bk1ng_94.pdf - denied.
  * Note the use of two positions for the group number.

<!-- * __All your source code has been compressed into a .zip archive__
  * Please make sure to clean your build directories before archiving. -->

<!-- * __Please verify your compressed code repository__ 
  * Please make sure your to be submitted code is being compiled and run perfectly with the baseline project, you can check the [following](https://github.com/acstud/lab1/blob/master/code%20submission%20instructions.sh) instructions. -->

<!-- * __The file name of your source code archive is GROUP_XX_netid0_netid1_netid2.zip__
  * Example: GROUP_01_alee_bzhang_cwang.zip - accepted.
  * Example: acslab1.tar.gz - denied. -->
  
* __They have been uploaded to Brightspace before the deadline has expired.__
  
  * In general, the deadline is entered in Brightspace and it is not 
  possible to upload stuff after the deadline.
  
  * Brightspace was down? E-mail your report on time to 
    [acs-ewi@tudelft.nl](mailto:acs-ewi@tudelft.nl).
    * Example: deadline is Monday Sep 17, 09:00 AM CEST. 
    You have taken ample time to finish the report without having to worry,
    and so your clock shows 08:59:59. You hit the send button. The course
    instructors check the time on the e-mail. It says 09:00:00 AM CEST. 
    The report is on time.
    * Example: deadline is Monday Sep 17, 09:00 AM CEST. 
        You have taken ample time to finish the report without having to worry,
        and so your clock shows 08:59:59. You hit the send button. The course
        instructors check the time on the e-mail. It says 09:00:01 AM CEST. 
        The report is not on time. You will not receive points or feedback.
    * Your e-mail was sent from a TU Delft (student) e-mail account.
        * Example: an e-mail sent from bzhang-1@student.tudelft.nl - accepted.
        * Example: an e-mail sent from 23476324@qq.com - denied.
        * Example: an e-mail sent from n00bk1ng_94@yahoo.com - denied.

## How is the lab graded?

Using your report and a rubric that gives indicators for grades based on
the report. You can find [this rubric here](rubric.pdf). This rubric is
used for all labs.

Remember: "Profiling the application" and "Quality of the report" count as 50% of your grade.

## Can I ask ChatGPT how to implement matrix multiplication with SIMD?

Sure. Here is the answer from ChatGPT (correctness not guaranteed). Remember you still need to analyze the performance and optimize your code!

Here is the code for the SIMD implementation.
```cpp
#include <iostream>
#include <immintrin.h>

void matrix_multiply(float *A, float *B, float *C, int N) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            __m256 c = _mm256_setzero_ps();
            for (int k = 0; k < N; k += 8) {
                __m256 a = _mm256_loadu_ps(&A[i * N + k]);
                __m256 b = _mm256_loadu_ps(&B[k * N + j]);
                c = _mm256_add_ps(c, _mm256_mul_ps(a, b));
            }
            float temp[8] __attribute__((aligned(32)));
            _mm256_store_ps(temp, c);
            C[i * N + j] = temp[0] + temp[1] + temp[2] + temp[3] + temp[4] + temp[5] + temp[6] + temp[7];
        }
    }
}

int main() {
    int N = 8;
    float A[] = { /* Your matrix A elements */ };
    float B[] = { /* Your matrix B elements */ };
    float C[N * N];

    matrix_multiply(A, B, C, N);

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            std::cout << C[i * N + j] << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

Here is the code for the OpenMP implementation.
```cpp
#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <omp.h>

void matrix_multiply(const std::vector<std::vector<float>> &A,
                     const std::vector<std::vector<float>> &B,
                     std::vector<std::vector<float>> &C,
                     int N) {
    #pragma omp parallel for
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            float sum = 0.0;
            for (int k = 0; k < N; k++) {
                sum += A[i][k] * B[k][j];
            }
            C[i][j] = sum;
        }
    }
}

int main() {
    int N = 8;
    std::vector<std::vector<float>> A(N, std::vector<float>(N, 1.0));
    std::vector<std::vector<float>> B(N, std::vector<float>(N, 1.0));
    std::vector<std::vector<float>> C(N, std::vector<float>(N, 0.0));

    matrix_multiply(A, B, C, N);

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            std::cout << C[i][j] << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

Here is the code for the OpenCL implementation.

First, create a new file called matrix_multiply.cl containing the OpenCL kernel code:
```cpp
__kernel void matrix_multiply(const __global float *A, const __global float *B, __global float *C, int N) {
    int i = get_global_id(0), j = get_global_id(1);
    float sum = 0.0f;
    for (int k = 0; k < N; k++) { sum += A[i * N + k] * B[k * N + j]; }
    C[i * N + j] = sum;
}

```
Then, write the main C++ code to set up the OpenCL environment and execute the kernel.
```cpp
#include <iostream>
#include <vector>
#include <fstream>
#include <CL/cl.hpp>

std::string read_kernel(const std::string &filename) {
    std::ifstream kernel_file(filename);
    return {std::istreambuf_iterator<char>(kernel_file), std::istreambuf_iterator<char>()};
}

int main() {
    int N = 8;
    std::vector<float> A(N * N, 1.0), B(N * N, 1.0), C(N * N, 0.0);
    cl::Device default_device = cl::Device::getDefault();
    cl::Context context({default_device});
    cl::CommandQueue queue(context, default_device);
    std::string kernel_code = read_kernel("matrix_multiply.cl");
    cl::Program program(context, {kernel_code.c_str(), kernel_code.length()});
    program.build({default_device});
    cl::Buffer buf_A(context, CL_MEM_READ_ONLY, sizeof(float) * N * N), buf_B(context, CL_MEM_READ_ONLY, sizeof(float) * N * N), buf_C(context, CL_MEM_WRITE_ONLY, sizeof(float) * N * N);
    queue.enqueueWriteBuffer(buf_A, CL_TRUE, 0, sizeof(float) * N * N, A.data());
    queue.enqueueWriteBuffer(buf_B, CL_TRUE, 0, sizeof(float) * N * N, B.data());
    cl::Kernel kernel(program, "matrix_multiply");
    kernel.setArg(0, buf_A); kernel.setArg(1, buf_B); kernel.setArg(2, buf_C); kernel.setArg(3, N);
    queue.enqueueNDRangeKernel(kernel, cl::NullRange, cl::NDRange(N, N), cl::NullRange);
    queue.enqueueReadBuffer(buf_C, CL_TRUE, 0, sizeof(float) * N * N, C.data());
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) { std::cout << C[i * N + j] << " "; }
        std::cout << std::endl;
    }
    return 0;
}
```

## What happens if I plagiarize?

Many bad things.

In general, if you plagiarize (read about it in the exam regulations) it
means we cannot grade you personally anymore, as it is not your work.

Therefore, as examinators, we cannot perform our job correctly. Thus, we
must consult the exam committee about the case. Depending on the severity
they can decide to prevent you from taking examinations all the way up to
expel you from the master program!

Sometimes, for source code it is hard to determine when it is plagiarism.
Therefore, observe the following examples:

* You find code that exactly solves one of the parts of the lab. You blindly
  copy-paste the code without modifications. After we talk to you about this, 
  it seems like you cannot explain the code either. You also haven't included
  a source. 
  * Our verdict: plagiarism.
  * Reason: it is not your work, so we cannot grade you
  * Consequence: case will be brought before exam committee.
  
* You find code that exactly solves one of the parts of the lab. You blindly
  copy-paste the code without modifications. After we talk to you about this, 
  it seems like you cannot explain the code either. You have included a 
  source.
  * Our verdict: not formally plagiarism, as you have stated your source,
   but your grade is zero because you didn't learn anything.
  * Reason: it is not your work, so we cannot grade you; you will not 
    receive any points.
  * Consequence: you will have to do some form of resit and not repeat
  this behaviour.
  
* You find code that partially solves one of the parts of the lab. You 
  copy-paste some parts of the code that are useful to your problem and 
  apply several modifications. You can explain how the code works through
  the report and include your sources of inspiration in the report or in
  your source code.
  * Our verdict: well done
  * Reason: you have searched the internet for interesting solutions and
  modified them to work within your own context. You also understand how
  and why it solves your problem.
  * Consequence: nothing, but you might want to upvote the source and 
  buy a StackOverflow t-shirt.

Obviously, similar examples could be given for the report, although 
do not copy any sentences from any sources, without citing them correctly.
Always completely formulate your own sentences, even if your language
skills are not what you wish they would be. We will also give feedback
in terms of language.

## Other questions? Brightspace down? E-mail?

You can e-mail [acs-ewi@tudelft.nl](mailto:acs-ewi@tudelft.nl). 
One of the TA's / course instructors will answer you A.S.A.P.

<!-- ## Can I talk face-to-face?

This year, the lab sessions will be carried out online using the Discord application. Please use the following link to join our [ACS Discord server](https://discord.gg/7unmM9cG33) -->

## Commonly seen OpenCL error codes

* OpenCL error: -1001
  * Are you running on a node that has a GPU?
