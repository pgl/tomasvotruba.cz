---
id: 79
title: "New in Symplify 3: 4 Improvements in EasyCodingStandard"
perex: |
    What is new in Easy Coding Standard 3?
    Nice diffs for fixable sniffs, smart excluding, support for sniff warnings and one more...
tweet: "New post on my blog: New in Symplify 3: 4 Improvements in EasyCodingStandard #codingstandard php"
tweet_image: "/assets/images/posts/2018/symplify-3-ecs/exclude-files.png"

updated_since: "December 2018"
updated_message: |
    Updated with <strong>EasyCodingStandard 5</strong>, Neon to YAML migration and new simplified `skip` parameter syntax.
---

## 1. Exclude Files or Dirs

Do you have `src/Migrations` that you need to skip from your `vendor/bin/ecs check src` command?

```yaml
# ecs.yml
parameters:
    exclude_files:
        - 'src/Migrations/LastMigration.php'
        # or better all files from the dir
        - '*src/Migrations/*.php'
```

With favorite [`fnmatch()` function](http://php.net/manual/en/function.fnmatch.php) on board.

## 2. Warnings are Reported for Specific Sniffs

<a href="https://github.com/Symplify/Symplify/pull/481" class="btn btn-dark btn-sm mb-3 mt-2">
    <em class="fab fa-github"></em>
    &nbsp;
    Check the PR #481
</a>

Sniff warnings are skipped by default, because it doesn't make sense to differentiate errors vs warnings. Yet some official Sniffs only produce warnings and that made them useless. Like `PHP_CodeSniffer\Standards\Squiz\Sniffs\PHP\CommentedOutCodeSniff`.

That changed. New property [`$reportWarningsSniffs` in `Symplify\EasyCodingStandard\SniffRunner\File\File`](https://github.com/Symplify/Symplify/blob/3d058becb57efefe2307c88ee94acbfbd15ebd1c/packages/EasyCodingStandard/packages/SniffRunner/src/File/File.php#L52) now lists all sniffs, that report warnings in ECS as well.

**Do you miss useful Sniff that reports only warnings?** Send PR to add it.

## 3. Nice and Clear Diff over Boring Table Report

<a href="https://github.com/Symplify/Symplify/pull/474" class="btn btn-dark btn-sm mb-3 mt-2">
    <em class="fab fa-github"></em>
    &nbsp;
    Check the PR #474
</a>

Inspired by [PHP CS Fixer](https://github.com/friendsofphp/php-cs-fixer) we've decided to **use files diffs everywhere wherever it saves user daunting reading**.

When a **fixable sniff found an error**, ECS reported it like this:

```bash
 ------ --------------------------------------------------------------------------------------------
  Line   src/Posts/Year2017/Ast/SomeClass.php
 ------ --------------------------------------------------------------------------------------------
  10     Property $someProperty should use doc block instead of one liner
         (SomeSniff)
```

But why bother with such detailed text information, if the ECS will fix it to better form anyway?

From now on, **it is reported the PHP CS Fixer-way like all the fixers**:

```diff
@@ -1,14 +1,13 @@
 final class SomeClass
 {
+    /**
+     * @var SomeType
+     */
-    /** @var SomeType */
     private $someProperty;
 }

    ----------- end diff -----------

Applied checkers:

 - SomeSniff
```

Which one do you prefer?

## 4. Skip Sniff Codes instead of Whole Sniffs

<a href="https://github.com/Symplify/Symplify/pull/388" class="btn btn-dark btn-sm mb-3 mt-2">
    <em class="fab fa-github"></em>
    &nbsp;
    Check the PR #388
</a>

If you wanted to skip specific part of sniff, you had to **exclude whole sniff** via `exclude_checkers` option:

```yaml
# ecs.yml
parameters:
    exclude_checkers:
        # to skip ".UselessDocComment"
        - 'SlevomatCodingStandard\Sniffs\TypeHints\TypeHintDeclarationSniff'
```

But what if you liked all the other codes? **Now you can:**

```yaml
# ecs.yml
parameters:
    skip:
        SomeScalarSniff.SpecificCode: ~
```

And all the other codes will be checked properly in your code.

<br>

<a href="https://github.com/Symplify/Symplify/pull/406" class="btn btn-dark btn-sm mb-3 mt-2">
    <em class="fab fa-github"></em>
    &nbsp;
    Check the PR #406
</a>

[@ostrolucky](https://github.com/ostrolucky) took this feature even further and added  **skipping by list of files or [`fnmatch`](http://php.net/manual/en/function.fnmatch.php)**.

```diff
 # ecs.yml
 parameters:
     skip:
-        SomeScalarSniff.SpecificCode: ~
+        SomeScalarSniff.SpecificCode:
+            - '*src/Form/Type/*Type.php'
```

Hope you like the changes and thanks the people who push these tools further by every single PR or issue report!

<br>

Happy code fixing!
