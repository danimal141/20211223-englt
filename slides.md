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

# I know everything about mocks, stubs, and spies in RSpec

<div class="m-20">
  <p>@danimal141</p>
  <p>2021/12/23 English Tech LT#2</p>
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

## See difference between about mocks, stubs, and spies using [rspec-mocks](https://github.com/rspec/rspec-mocks)!

---
layout: center
---

## Japanese kanzen rikai!

<br />

<img src="/kanzen_rikai.jpeg" width=800 />

---
layout: center
---

## Let's think with some examples...

---
layout: center
---

## Stubs

---

There is some API client wrapper.

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
  # instance_double:
  #   - Can stub methods.
  #   - If you try to stub the undefined method, it would raise an error.
  let(:some_lib) { instance_double(SomeLib) }
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  # call the subject.
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
    # ensure whether it's really called once expectedly.
    # set message expectations
    # call the expectation first.
    expect(some_lib).to receive(:request).with(no_args).and_return(expected_boby).once
    # call the subject.
    is_expected.to eq expected_json
  end
```

---
layout: center
---

## Spies

---

<br />

In [rspec-mocks/v/3-6/docs/basics/spies](https://relishapp.com/rspec/rspec-mocks/v/3-6/docs/basics/spies), Spies are described as follows:

> Spies are an alternate type of test double that support this pattern by allowing you to expect that a message has been received after the fact, using have_received.

---

Test with spies.

<br />

```ruby
describe 'SomeClient#request'
  before { allow(client).to receive(:some_lib).with(no_args).and_return(some_lib) }

  let(:client) { SomeClient.new }
  let(:some_lib) { instance_double(SomeLib) }
  let(:expected_body) { ... }
  let(:expected_json) { JSON.parse expected_body }

  subject { client.request }

  it do
    # call the subject first.
    is_expected.to eq expected_json
    # set message expectations
    # call the expectation after calling the subject.
    expect(some_lib).to have_received(:request).with(no_args).and_return(expected_boby).once
  end
end
```

---

## Conclusion

<br />

- stub
  - Provides hard-coded values for method calls.
- mock
  - mock is similar to stub, but represents the specification of the call at tests.
  - set message expectation -> test
- spy
  - test -> verify message expectation
  - Can write tests more naturally.

---
layout: center
---

## Thanks!
