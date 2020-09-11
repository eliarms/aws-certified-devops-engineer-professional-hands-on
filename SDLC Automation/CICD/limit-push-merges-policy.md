# CodeCommit - Securing the repositories and Branch

By default, any CodeCommit repository user who has sufficient permissions to push code to the repository can contribute to any branch in that repository. This is true no matter how you add a branch to the repository: by using the console, the command line, or Git. However, you might want to configure a branch so that only some repository users can push or merge code to that branch. For example, you might want to configure a branch used for production code so that only a subset of senior developers can push or merge changes to that branch. Other developers can still pull from the branch, make their own branches, and create pull requests, but they cannot push or merge changes to that branch. You can configure this access by creating a conditional policy that uses a context key for one or more branches in IAM.

**Configure an IAM policy to limit pushes and merges to a branch**

You can create a policy in IAM that prevents users from updating a branch, including pushing commits to a branch and merging pull requests to a branch. To do this, your policy uses a conditional statement, so that the effect of the Deny statement applies only if the condition is met. The APIs you include in the Deny statement determine which actions are not allowed. You can configure this policy to apply to only one branch in a repository, a number of branches in a repository, or to all branches that match the criteria across all repositories in an AWS account.

```
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Deny",
     "Action": [
       "codecommit:GitPush",
       "codecommit:DeleteBranch",
       "codecommit:PutFile",
       "codecommit:MergeBranchesByFastForward",
       "codecommit:MergeBranchesBySquash",
       "codecommit:MergeBranchesByThreeWay",
       "codecommit:MergePullRequestByFastForward",
       "codecommit:MergePullRequestBySquash",
       "codecommit:MergePullRequestByThreeWay"
     ],
     "Resource": "arn:aws:codecommit:*:*:*",
     "Condition": {
       "StringEqualsIfExists": {
         "codecommit:References": [
           "refs/heads/master"
         ]
       },
       "Null": {
         "codecommit:References": false
       }
     }
   }
 ]
}

```

ref: https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-conditional-branch.html
