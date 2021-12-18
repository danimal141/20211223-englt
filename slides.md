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

# Can you explain what mocks, stubs, and spies are?

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
    expect(some_lib).to receive(:request).with(no_args).and_return(expected_boby).once # Ensure it's really called expectedly.
    is_expected.to eq expected_json
  end
```

---
layout: center
---

## Spies

---

Test with spies.

<br />

```ruby
describe 'SomeClient#request'
  before { allow(client).to receive(:some_lib).with(no_args).and_return(some_lib)  }

  let(:client) { SomeClient.new }
  let(:some_lib) { instance_spy(SomeLib) }
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  it do
    is_expected.to eq expected_json
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

I found an article: [https://www.futurelearn.com/info/blog/stubs-mocks-spies-rspec](https://www.futurelearn.com/info/blog/stubs-mocks-spies-rspec), and there is what I would like to say:

<br />

> Stub – an object providing canned responses to specified messages.

> Mock – an object that is given a specification of the messages that it must receive (or not receive) during the test if the test is to pass.

> Spy – an object that records all messages it receives (assuming it is allowed to respond to them), allowing the messages it should have received to be asserted at the end of a test.

---
layout: center
---

## Thanks!
