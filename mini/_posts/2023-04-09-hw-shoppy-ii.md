---
layout: post
title: HW-SHPPOY [struggle]
image:
  path: /assets/img/mini/hw-shoppy/proceduralprogramming.png
description: >
  [hw-shoppy] api 모듈 정의에 대하여
sitemap: false
---

[hw-shoppy] 프로젝트를 진행하면서 api 모듈을 어떻게 분리하고 작성할지에 대해 고민해보았다.✍
{:.lead}

[Modernized](#linking-in-style) [design](#whats-in-the-cards), [big headlines](#ready-for-the-big-screen), big new features: [Built-In Search](#built-in-search), [Sticky Table of Contents](#sticky-table-of-contents), and [Auto-Hiding Navbar](#auto-hiding-navbar). That [and more](#and-much-more) is Hydejack 9.

- api 기능별 함수로 분리하여 정의하기
  {:toc .large-only}
- 함수형 api 에서 객체 api 모듈로
  {:toc .large-only}

## api 기능별 함수로 분리하여 정의하기

기능별 함수로 분리하여 사용하는 것은 기능이 생길때마다 함수를 계속 생성해줘야했다.
함수로 생성하다보니 코드가 커지게 되면서 여러 함수를 관리하는 것이 복잡해졌다.

firebase로 api 호출을 하다보니 firebase database를 반복적으로 조회하는 부분들이 생겨났다.
예를 들면 나의 관심 목록 전체를 가져오는 api 와 상품 상세 화면에서 동일하게 collection(db,"interest") 를 생성하여 조회하고 있었다...

또한,상품 id를 통한 나의 관심 상품인지 파악하는 것과 나의 관심목록 전체를 가져오는 함수를 따로 빼는 것보다 하나의 함수에서 id를 flag 값으로 조회하는 것이 훨씬 코드의 양을 줄이고 가독성을 높일 수 있을 것이다.

이러한
함수 단위로 코드를 분리하고 재사용하는 형태인 [절차적(구조적) 프로그래밍](https://yozm.wishket.com/magazine/detail/1396/) 보다 비슷한 유형의 코드를 하나의 group으로 묶어서 생각하는 방식 [객체 재향 프로그래밍](https://yozm.wishket.com/magazine/detail/1396/)으로 변경의 필요성을 느끼게 되었다.

## 함수형 api 에서 객체 api 모듈로

함수형 api 에서 공통적으로 쓰이는 부분은 1. firebase database 에서 데이터를 조회 2. 조회해온 데이터를 포맷팅 이다.

데이터만을 조회해오는 class 인 ShopClient 와 ShopClient에서 조회해온 데이터를 포맷팅하는 Shop class로 나눠서 구현하였다.

## What's in the Cards?

Hydejack 9 now lets you use content cards for both projects and posts.
The cards have been redesigned with a new hover style and drop shadows and they retain their unique transition-to-next-page animations, which now also work on the `blog` layout. The new `grid` layout lets you do that.

Good images are key to making the most out of content cards. For that reason, a [chapter on images](../../docs/basics.md#adding-images) has been added to the documentation.

## Built-In Search

Hydejack now has Built-In Search. It even works offline. I've been prototyping many approaches and eventually settled on a fully client-side, off-the-main thread solution that perfectly fits the use case of personal sites and shows surprisingly good results[^2].

The new search UI is custom made for Hydejack and shows beautiful previews of your posts and pages, right on top of your regular content.

Together with the Auto-Hiding Navbar, your entire content library is now only a couple of keystrokes away.

## Auto-Hiding Navbar

A navbar that's there when you need it, and disappears when you don't. Simple as that.

## Sticky Table of Contents

Already a staple on so many sites on the web, this pattern is now also available in Hydejack.
What's unique about it is that it simply picks up the table of contents already created by kramdown's `{:toc}` tag and transparently upgrades it to a fully dynamic version.

## …and much more

Other noteworthy changes include:

- Support for Jekyll 4
- Choice between MathJax and KaTeX for math rendering
- Use of `jekyll-include-cache` for drastically improved page building speeds
- New variables configuration file — adjust content width, sidebar width, font size, etc...
- ...and the option to disable grouping projects by year.

Read the the [CHANGELOG](../../CHANGELOG.md){:.heading.flip-title} for the full scope of features and improvements made in Hydejack 9.
Maybe just glance at it to confirm that it is indeed a pretty long list.

## Even More to Come

New features for 9.1 are already lined up. Code block headers and code line highlights for even better technical blogging, as well as download buttons on the resume page for PDF, vCard, and Resume JSON are just around the corner.

## Get It Now

The Free Version of Hydejack is now availabe on [RubyGems](https://rubygems.org/gems/jekyll-theme-hydejack)
and for the first time also on [GitHub Packages](https://github.com/hydecorp/hydejack/packages).
The source code is available on [GitHub](https://github.com/hydecorp/hydejack) as always.

The PRO Version is scheduled to release on July 7th on Gumroad. Pre-Orders are open now:

<div class="gumroad-product-embed" data-gumroad-product-id="nuOluY"><a href="https://gumroad.com/l/nuOluY">Loading…</a></div>

[^1]: If you are a fan of the old two-column layout, or don't like modern design tropes such as mega headlines, Hydejack lets you revert these changes on a case-by-case basis via configuration options.
[^2]:
    Search was mainly tested for English and German. Please let me know about issues in other languages.
    While I've tried to find a multi-language solution, most showed drastically worse results for the English base case.
    If you're technically inclined, you can adopt the code located in `_includes/js/search-worker.js` to your needs.
