# CircleCI Lunch & Learn
<img src="images/continuous-integration-with-circle-ci.png">

# CircleCI 101 - Lunch & Learn  <img src="images/lunch-learn-brown-bag-apple-notebooks.jpg" width="50" height="50" />

## Prereqs 

* Some basic knowledge of git and an existing GitHub.com account *(also fine for people to create an account at the beginning of class)*
* Some basic terminal or bash know-how is helpful. Prior experience using the command line comes in handy. We will be using a JavaScript project in our example. But no worries, there is no need to know all the ins-and-outs of JavaScript :relieved:
* It is necessary to have your GitHub.com SSH Keys setup for the SSH-ing into your build section. The information you need for that is [here](https://help.github.com/articles/connecting-to-github-with-ssh/)

## What is Continuous Integration? 

**Continuous Integration** is a practice that encourages developers to integrate their code into a `master` branch of a shared repository early and often. Instead of building out features in isolation and integrating them at the end of a development cycle, code is integrated with the shared repository by each developer multiple times throughout the day.

**Continuous Integration** is a key step to digital transformation.

**What?**    
Every developer commits daily to a shared mainline.  
Every commit triggers an automated build and test.  
If build and test fails, it’s repaired quickly - within minutes.  

**Why?**    
Improve team productivity, efficiency, happiness.
Find problems and solve them, quickly.
Release higher quality, more stable products.

## CircleCI

**CircleCI** - Our mission is to empower technology-driven organizations to do their best work.  
We want to make engineering teams more productive through intelligent automation.

## Advantages of Continuous Integration

Continuous Integration allows organizations to: 
* Improve team productivity and efficiency
* Accelerate time to market
* Release higher quality, more stable products
* Increase customer satisfaction
* Keep developers happy, and shipping code

*CircleCI provides Enterprise-class support + services, with the flexibility of a startup.  
We work where you work: Linux, macOS, Android - SaaS or behind your firewall.  
Leverage the opportunities created by your modern Git repos.  
Set up in minutes out of the box, or fully customize to suit your needs.* 

## First CircleCI Build
#### :computer: Let's try out something simple to start off with
### Creating a repository 
* Navigate to your account on GitHub.com 
  * Go to the **Repositories** tab and then select **New**
  * Alternatively you can navigate directly to https://github.com/new
<img src="images/GH_Repo-New-Banner.png">
<img src="images/create-repo-circle-101-initialise-readme.png">

### Adding a .yml file

CircleCI uses a [YAML](https://en.wikipedia.org/wiki/YAML) file to identify how you want your testing environment setup and what tests you want to run.
On CircleCI 2.0, this file must be called `config.yml` and must be in a hidden folder called `.circleci` (on Mac, Linux, and Windows systems, files and folders whose names start with a period are treated as system files that are hidden from users by default).

 * To create the file and folder on GitHub, click the **"Create new file"** button the repo page and type `.circleci/config.yml`.
  
 * You should now have in front of you a blank `config.yml` file in a `.circleci` folder.

* To start out with a simple config.yml, copy the text below into the file editing window on GitHub:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
```
      
The `- image: circleci/ruby:2.4.1` text tells CircleCI what Docker image to use when it builds your project. Circle will use the image to boot up a "container" — a virtual computing environment where it will install any languages, system utilities, dependencies, web browsers, etc., that your project might need in order to run.   (CircleCI provides images for most every language)[https://circleci.com/docs/2.0/circleci-images/] based on populare community images.

### Setting up your build on CircleCI

For this step, you will need a CircleCI account. Visit https://circleci.com/signup and click "Start with GitHub". You will need to give CircleCI access to your GitHub account in order to run your builds. 

If you already have a CircleCI account then you can navigate to your dashboard: https://circleci.com/dashboard

Next, you will be given the option of "following" any projects you have access to that are already building on CircleCI (this would typically apply to developers connected to a company or organization's GitHub/Bitbucket account). Since this probably doesn't apply to you, click "Skip - I don't want to follow any projects." On the next screen, you'll be able to add the repo you just created as a new project on Circle.

To add your new repo, ensure that your GitHub account is selected in the dropdown in the upper-left, find the repository you just created below, and click the "Setup project" button next to it.

<img src="images/CircleCI-add-new-project-list.png">

On the next screen, you're given some options for configuring your project on CircleCI.  The options help you generate a sample config.yml yo start with.  For now leave everything as-is for now and just click the "Start building" button a bit down the page on the right.

<img src="images/CircleCI-2.0-start-building.png">

### Running your first CircleCI build!

You should see your build start to run automatically—and pass! So, what just happened? Click on the green button and let's investigate.

1. **Spin up environment:** CircleCI used the `circleci/ruby:2.4.1` Docker image to launch a virtual computing environment.

2. **Checkout code:** CircleCI checked out your GitHub repository and "cloned" it into the virtual environment launched in step 1

3. **echo:** this was the only other instruction in your `config.yml` file: CircleCI ran the echo command with the input "A first hello" ([echo](https://linux.die.net/man/1/echo) does exactly what you'd think it would do)

Even though there was no actual source code in your repo, and no actual tests configured in your `config.yml`, CircleCI considers your build to have "succeeded" because all steps completed successfully (returned an [exit code](https://en.wikipedia.org/wiki/Exit_status) of 0). Most customers' projects are far more complicated, oftentimes with multiple Docker images and multiple steps, including a large number of tests—here's an example. You can learn more about all the possible steps one might put in a `config.yml` file [here](https://circleci.com/docs/2.0/configuration-reference).

### Breaking your build!

Edit your `config.yml` file (you can just do this in the GitHub editor for simplicity) and replace `echo "A first hello"` with `notacommand`. Commit and push this change (or just hit "Commit" in the GitHub editor) to trigger a new build and see what happens!


### Using Workflows

To see Workflows in action we can edit our `.circle/config.yml` file. Once you have the file in edit mode in your browser window, select the text from `build` and onwards in you file and copy and paste the text to duplicate that section.

That should look similar to the code block below:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
  test:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"      
```

Next we need to rename our two builds so that they have different names. In my example below I imaginatively picked `build` and `test`. Change the contents of the echo statements to something different. To make the build take a longer period of time we can add a system sleep command. 

We need to add a `workflows` section to our config file. The workflows section can be placed anywhere in the file. Typically it is found either at the top or the bottom of the file. 


```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
      - run: sleep 5
  test:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A more familiar hi"
      - run: sleep 5
workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - test
```

Commit these changes to your repository and navigate back over to the CircleCI dashboard. 

<img src="images/workflows-circle-101-running.png">

And drilling a little deeper into our workflow...

<img src="images/inside-workflows-circle-101-running.png">

Since we want our jobs to run sequentially, we add the `requires` directive.


```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
      - run: sleep 5
  test:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A more familiar hi"
      - run: sleep 5
workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - test:
          # only run this job if build succeeds
          requires:
            - build 
```

### Fan-out / Fan-in Workflows
Sometimes you'll have more lengthy jobs (integration or browser testing) that can be broken into parallel tracks. If all these jobs pass, you can merge back into final steps (like deployment).  This technique is commonly reffered to as fan-out/fan-in.

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
      - run: sleep 5
      
      
  testa:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A more familiar hi"
      - run: sleep 5
  testb:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A localized Salut"
      - run: sleep 5
      
      
  deploy:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A final goodbye"
      - run: sleep 5
      
      
workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - testa:
          requires:
            - build 
      - testb:
          requires:
            - build 
      - deploy:
          requires:
            - testa
            - testb
```

You can read more about workflows here: https://circleci.com/docs/2.0/workflows/#overview



### Adding some changes to use Workspaces 

Each Workflow has an associated Workspace which can be used to transfer files to downstream jobs as the workflow progresses. You can use workspaces to pass along data that is unique to this run and which is needed for downstream jobs. Try updating `config.yml` to the following:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: mkdir -p my_workspace
      - run: echo "Hello World" > my_workspace/echo-output
      - persist_to_workspace:
          # Must be an absolute path, or relative path from working_directory
          root: my_workspace
          # Must be relative path from root
          paths:
            - echo-output      
  testa:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - attach_workspace:
          # Must be absolute path or relative path from working_directory
          at: my_workspace

      - run: cat my_workspace/echo-output
          
  testb:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - attach_workspace:
          # Must be absolute path or relative path from working_directory
          at: my_workspace

      - run: |
          # this will fail intentionally, we'll use SSH to debug and fix
          if [[ $(cat my_workspace/echo-output) == "Trying out workspaces" ]]; then
            echo "It worked!";
          else
            echo "Nope!"; exit 1
          fi   
          
  deploy:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - attach_workspace:
          # Must be absolute path or relative path from working_directory
          at: my_workspace

      - run: |
          echo "Deploying message!"
          cat my_workspace/echo-output

workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - testa:
          requires:
            - build 
      - testb:
          requires:
            - build 
      - deploy:
          requires:
            - testa
            - testb
```

**NOTE**: Uh-oh, our `testb` job failed, blocking our deployment.  Can you use SSH debugging (below) to find the right fix?  Hint: Try to run the `at my_workspace/echo-output` yourself, what is the output?
     
     
You can read more about Workspaces here: https://circleci.com/docs/2.0/workflows/#using-workspaces-to-share-data-among-jobs

### SSH'ing into your build

<img src="images/SSH-screen.jpg" width="100" height="100" />

For those who are comfortable with the terminal, you can SSH directly into your CircleCI jobs to troubleshoot issues with your builds by rerunning your build with the SSH enabled option. 

*Note that you will need to add your SSH keys to your GitHub account:
https://help.github.com/articles/connecting-to-github-with-ssh/*

<img src="images/rebuild-with-SSH.png">

<img src="images/SSH-build-terminal-string.png">

Copy the `ssh` string from the enabling SSH section of your build. Open a terminal and paste in the `ssh` string. 

Using some of the following commands see if you can find and view the contents of the file we created using workspaces

```
pwd     #  print what directory, find out where you are in the file system
ls -al   # list what files and directories are in the current directory
cd <directory_name>    # change directory to the <directory_name> directory 
cat <file_name>    # show me the contents of the file <file_name>
```

## Further resources/links :link:

Blog post on how to validate the CircleCI `config.yml` on every commit with a git hook - *extra credit* :apple:
https://circleci.com/blog/circleci-hacks-validate-circleci-config-on-every-commit-with-a-git-hook/

### CircleCI

* The CircleCI blog and how to follow it
  * https://circleci.com/blog/
* Relavant blog post  
  * https://circleci.com/blog/what-is-continuous-integration/
* Our other social media and GitHub
  * https://github.com/circleci
  * https://twitter.com/circleci
  * https://www.facebook.com/circleci
  
### Continuous Integration

* https://martinfowler.com/articles/continuousIntegration.html
* https://en.wikipedia.org/wiki/Continuous_integration#Best_practices
  
### YAML
* https://en.wikipedia.org/wiki/YAML#Advanced_components

### Terraform
* https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180

