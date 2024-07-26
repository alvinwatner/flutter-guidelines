<p align="center"><a href="https://jobseeker.company" target="_blank"><img src="https://assets.jobseeker.software/uploads/employer/logo/2023-03/dot-company-logo.png" width="400"></a></p>


_(last updated 5/12/203)_

# How to contribute : 

There are many ways you can contribute to our lovely codebase. Code contributions are not the only way to help our team, but :

* Improve docs and this guidelines
* Improve code readbility
* Improve UI 
* Add a new test
* Create an issue 
* Fix an issue
* Add new features
* ...and many more!

All are equally valuable to the team. 


## Code Style Guidelines 

Before writing codes, check out our [styling guidelines](https://github.com/Jobseeker-company/mobile-internal-docs/blob/master/STYLE.md) first.


## Start Contributing! (Pull Requests) :

You will need basic git proficiency to be able to contribute. Follow these steps to start contributing:
1. Fork the repository by clicking on the ‘Fork’ button on the repository’s page. This creates a copy of the code under your GitHub user account.
2. Clone your fork to your local disk, and add the base repository as a remote:
    ```
    $ git clone git@github.com:<your Github handle>/mobile-b2c-app-reborn.git
    $ cd mobile-b2c-app-reborn
    $ git remote add upstream https://github.com/mobile-b2c-app-reborn.git
    ```
3. Create a new branch to hold your development changes:
   ```
   $ git checkout -b a-descriptive-name-for-my-changes
   ```
🚨 do not work on the master branch.

4. Start writing your code

    Once you’re happy with your changes, add changed files using git add and make a commit with git commit to record your changes locally:
   ```
    $ git add modified_file.dart
    $ git commit
   ```

    Please write [good commit messages](https://chris.beams.io/posts/git-commit/).

5. It is a good idea to sync your copy of the code with the original repository regularly. This way you can quickly account for changes:
   ```
    $ git pull upstream/development
   ```   
6. Push the changes to your account using:
   ```
    $ git push -u origin a-descriptive-name-for-my-changes
   ```      
7. Once you are satisfied (and the checklist below is happy too), go to the webpage of your fork on GitHub. Click on ‘Pull request’ to send your changes to the project maintainers for review.


## Checklist
☐ The title of your pull request should be a summary of its contribution\
☐ If your pull request adresses an issue, please mention the issue number in the pull request description to make sure they are linked (and people consulting the issue know you are working on it)\
☐ Make sure existing tests pass\
☐ Add high-coverage tests. No quality testing = no merge\
☐ All public methods must have informative docstrings. See example.dart for an example\

## Build Binary 🤖📱🍎

### APK (Android)
* Production environment
``` console
flutter build apk --flavor production --target lib/main_production.dart --split-per-abi --release
```
* Development environment
``` console
flutter build apk --flavor development --target lib/main_development.dart --split-per-abi --release
```

### App Bundle (Android)

``` console
flutter build appbundle --flavor production --target lib/main_production.dart --release
```

### IPA (iOS)

``` console
flutter build ipa --flavor production --target lib/main_production.dart --release
```

## Release for QA 🧪

### APK (Android)
  ``` console
  git checkout development  
  ```
  ``` console
  flutter build apk --flavor production --target lib/main_production.dart --split-per-abi --release
  ```

### Test Flight (iOS)

1. Build ipa from master branch (production)
``` console
  git checkout master
```
``` console
  flutter build ipa --flavor production --target lib/main_production.dart --release  
```  
2. Upload to Test Flight using [Transporter](https://apps.apple.com/us/app/transporter/id1450874784?mt=12).

## Release to production 🚀

1. Merge from branch master -development, not vice versa (development -master ❌). This is to prevent a mixed-up of development interaction with real user data in the production environment to the analytics tooling (e.g., UX Cam, Firebase Analytics).
2. Build app bundle and ipa from master branch (production).
   
### App Bundle (Android)

Ensure the keystore and key.properties exist and correctly placed.
``` console
flutter build appbundle --flavor production --target lib/main_production.dart --release
```

### Test Flight (iOS)
``` console
  git checkout master
```
``` console
  flutter build ipa --flavor production --target lib/main_production.dart --release  
```  

## Folder Structures (Package-Level)

```
my_package
└─ lib
  └─ config
  └─ core
  └─ features
    └─ data
    └─ domain
    └─ presentation
  └─ main.dart    
└─ test    
  └─ core
  └─ features
    └─ data
    └─ domain
    └─ presentation
  └─ fixture
  └─ helper      
```
## Folder Structures (Feature-Level)

```
  ─ data
    └─ datasources
      └─ {feature}_local_datasource.dart
      └─ {feature}_remote_datasource.dart
    └─ models
      └─ {feature}
        └─ model1.dart
        └─ model2.dart        
    └─ repositories   
      └─ {feature}    
        └─ {feature}_repository.dart    
  ─ domain
      └─ repositories      
         └─ {feature}
           └─ {feature}_repository.dart         
      └─ usecases
         └─ {usecase}
           └─ {usecase}.dart            
  ─ presentation
      └─ bloc
      └─ ui
         └─ {feature}
           └─ helpers
             └─ {feature}_helpers.dart                       
           └─ pages
             └─ {feature}_page.dart                            
           └─ widgets           
      └─ widgets
```


Presentation layer folder explanation : 
1. bloc = all blocs across features
2. ui = user interface for every features
3. helpers = helper function for one specific feature
4. pages = all pages for one specific feature
5. widgets =  all extracted widget for one specific feature
