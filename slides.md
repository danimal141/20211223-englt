---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
---

# I completely understand mocks, stubs, and spies.

<div class="m-20">
  <p>@danimal141</p>
  <p>2021/12/23 English Tech LT #2</p>
</div>

---

## Introduction

<br />

- ID: @danimal141
- Name: Hideaki Ishii
- Work as a software engineer for about 10 years.
- I like Ruby, Golang, TypeScript.
- Lately, I use GraphQL ([graphql-ruby](https://github.com/rmosolgo/graphql-ruby)).

---
layout: center
---

## What mocks, stubs, and spies are?

---
layout: center
---

<img src="/kanzen_rikai.jpeg" width=800 />

---

## Martin Fowler says...

<br />

In [https://martinfowler.com/articles/mocksArentStubs.html](https://martinfowler.com/articles/mocksArentStubs.html), Mr. Martin Fowler says:

> Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.

> Spies are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.

> Mocks are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.

---
layout: center
---

## Let's think with some examples...

---
layout: center
---

## Stubs

---

There is some API client.

<br />

```ruby
class SomeClient
  def request
    body = some_lib.request
    JSON.parse body
  end

  private

  def some_lib
    @some_lib ||= SomeLib.new
  end
end
```
---

Test with stubs.

<br />

```ruby
describe 'SomeClient#request'
  before do
    allow(client).to receive(:some_lib).with(no_args).and_return(some_lib)
    # Stub some_lib's behavior and can avoid requesting actually.
    allow(some_lib).to receive(:request).with(no_args).and_return(expected_boby)
  end

  let(:client) { SomeClient.new }
  let(:some_lib) { instance_double(SomeLib) }
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  it { is_expected.to eq expected_json }
end
```

---
layout: center
---

## Mocks

---

Test with mocks.

<br />

```ruby
describe 'SomeClient#request'
  before { allow(client).to receive(:some_lib).with(no_args).and_return(some_lib)  }

  let(:client) { SomeClient.new }
  let(:some_lib) { instance_double(SomeLib) }
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  it do
    # Ensure whether it's really called once expectedly.
    expect(some_lib).to receive(:request).with(no_args).and_return(expected_boby).once
    is_expected.to eq expected_json
  end
```

---
layout: center
---

## Spies

---

<br />

In [https://relishapp.com/rspec/rspec-mocks/v/3-6/docs/basics/spies](https://relishapp.com/rspec/rspec-mocks/v/3-6/docs/basics/spies), Spies are described:

> Message expectations put an example's expectation at the start, before you've invoked the code-under-test. Many developers prefer using an act-arrange-assert (or given-when-then) pattern for structuring tests. Spies are an alternate type of test double that support this pattern by allowing you to expect that a message has been received after the fact, using have_received.

---

Test with spies.

<br />

```ruby
describe 'SomeClient#request'
  before { allow(client).to receive(:some_lib).with(no_args).and_return(some_lib)  }

  let(:client) { SomeClient.new }
  let(:some_lib) { instance_spy(SomeLib) } # instance_double can be also used :)
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  it do
    is_expected.to eq expected_json
    # Check whether it was really called once expectedly.
    expect(some_lib).to have_received(:request).with(no_args).and_return(expected_boby).once
  end
end
```

---

## instance_double / instance_spy

<br />

- instance_double
  - Can stub methods.
  - If you try to stub the undefined method, it would raise an error.
- instance_spy
  - Mix stubs and real objects.
  - If some real method is not stubbed, the real one would be called.
  - If you try to stub the undefined method, it would raise an error.

---

## Conclusion

<br />

- stub
  - Provides hard-coded values for method calls.
  - Always returns the same output, regardless of the input.
- spy
  - A stub that records information based on calls.
  - With spies, we can verify which method was called, with which arguments, when, and how many times.
- mock
  - mock is similar to stub, but represents the specification of the call at tests.
  - An exception can be thrown when a call is not expected.

---
layout: center
---

## Thanks!
