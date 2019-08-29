# TestRailStepRecovering
Test Rail step definition recovering from Edit History.

## Table of contents
* [Actual Problem](#actual-problem)
* [Technologies](#technologies)
* [An Idea](#an-idea)
* [Integration with Collection Runner](#integration-with-collection-runner)
* [Metrics](#metrics)

## Actual Problem
Sometimes configuration of Custom Steps in Test Rails happens. (http://docs.gurock.com/testrail-faq/config-steps)
And as you can see, notation about removing of all previous stored step definitions shown below whole instruction. 
In case of it, we faced problem when steps of all our (around 5k) tests was removed. 
Of course we have no backup. (haha, classic) 
Backups only for fools.
As we know steps was stored only on history of test. Becouse we have no access to testrails database as well.

After some investigation and reading of stackoverflow/google/testrails api docs we understood that there is no way to tak steps from history via API.

## Technologies
Project is created with:
* Postman: 7.5.0
* TestRail API: 2.0.0
* Cheerio

## An Idea
We have an idea. Thanks [@dmitalexandrov](https://github.com/dmitalexandrov). 
What if we just parse history page of test case and take last step changing result and push it via TestRail Api method called 'update_case'?

We used Postman(https://www.getpostman.com/) for it. Inside of postman we have pre-request script which just get html from history page.
After this we use Cheerio https://cheerio.js.org/ to parse html and find Steps definition on page with help of CSS (and jQuery) selectors.

So, we fetch steps from history. And just sent it to API.
Thats all.


## Integration with Collection Runner

We have around 5k tests, as I wrote above. Of course we just paste case ids of all manually and send requests. HAHA, NO.
We just export all of ours Case Ids from TestRails as csv file and change column name.
Start Collection Runner and push this file as data file. Thats all.

Format of file:

| caseId             |
|--------------------|
| there is case id   |

## Metrics

Updating of our ~5k test took like 1.5h. 
I think thats good result.
Anyway it's still better than manually rewrite it.

Average elapsed time of one request (and pre-request script) = 450 ms.
