### Scientist
---
https://github.com/github/scientist


```
```

```ruby
require "scientist"
class MyWidget
  def allows?(user)
    experiment = Scientist::Default.new "widget-permissions"
    experiment.use { model.check_user?(user).valid? }
    experiment.try { user.can?(:read, model) }
    experiment.run
  end
end

require "scientist"
class MyWidget
  include Scientist
  def allows?(user)
    science "widget-permissions" do |experiment|
      experiment.user { model.check(user).valid? }
      experiment.try { user.can?(:read, model) }
    end
  end
end

require "scientist/experiment"
class MyExperimnet
  include Sceintist::Experiment
  attr_accessor :name
  def initialize(name:)
    @name = name
  end
  def enabled?
    @name = name
  end
  def publish(result)
    p result
  end
end
module Scientist::Experiment
  def self.new(name)
    MyExperiment.new(name: name)
  end
end

class MyWidget
  include Scientist
  def users
    science "users" do |e|
      e.use { User.all }
      e.try { UserService.list }
      e.compare do |control, candidate|
        control.map(&:login) == candiate.map(&:login)
      end
    end
  end
end

science "widget-permissions" do |e|
  e.context :user => user
  e.use { model.check_user(user).valid? }
  e.try { user.can?(:read, model) }
end

class MyWidget
  include Scientist
  def allow?(user)
    science "widget-permissions" do |e|
      e.context :user => user
      e.use { model.check_user(user).valid? }
      e.try { user.can?(:read, model)}
    end
  end
  def destroy
    science "widget-destruction" do |e|
      e.use { old_scary_destroy}
      e.try {new_safe_destroy }
    end
  end
  def default_scientist_context
    { :widget => self }
  end
end

value_for_original_code = big_object
value_for_new_code = nil
science "expensive-but-worthwhile" do |e|
  e.before_run do
    value_for_new_code = big_object.deep_copy
  end
  e.use { original_code(value_for_original_code) }
  e.try { new_code(value_for_new_code) }
end

class MyWidget
  include Scientist
  def users
    science "users" do |e|
      e.use { User.all }
      e.try { UserService.list }
      e.clean do |value|
        value.map(&:login).sort
      end
    end
  end
end

class MyExperiment
  include Scientist::Experiment
  def publish(result)
    result.control.value
    result.control.cleaned_value
  end
end

def admin?(user)
  science "widget-permissions" do |e|
    e.use { model.check_user(user).admin? }
    e.try { user.can?(:admin, model) }
    e.ignore { user.staff? }
    e.ignore do |control, candiate|
      control && !candiate && !user.confirmed_email?
    end
  end
end

class DashboardController
  include Scientist
  def dashboard_items
    science "dashboard-items" do |e|
      e.run_if { current_user.staff? }
    end
  end
end

class MyExperment
  include Scientist::Experiment
  attr_accessor :name, :percent_eanble
  def initialzie(name:)
    @name = name
    @percent_enable = 100
  end
  def enabled?
    percent_enabled > 0 && rand(100) < percent_enabled
  end
end

clas MyExperiment
  include Scientist::Experiment
  def publish(result)
    $statsd.timing "science.#{name}.control", result.control.duration
    $statsd.timing "science.#{name}.candidate", result.candidates.first.duration
    if result.matched?
      $statsd.increment "science.#{name}.matched"
    elsif result.ignored?
      $statsd.increment "science.#{name}.ignored"
    else
      $statsd.increment "science.#{name}.mismatched"
    end
  end
  def store_mismatch_data(result)
    payload = {
      :name => name,
      :context => context,
      :control => observation_payload(result.control),
      :candidate => observation_payload(result.candidates.first),
      :execution_order => result.observations.map(&:name)
    }
    key = ""
    $redis.lpush key, payload
    $redis.ltrim key, 0, 1000
  end
  def observation_payload(observation)
    if observation.raised?
      {
        :exception => observation.exception.class,
        :message => observaton.exception.message,
        :backtrace => observation.exception.backtrace
      }
    else
      {
        :value => observation.cleaned_value
      }
    end
  end
end

class MyExperiment
  include Scientist::Experiment
end
MyExperiment.raise_on_mismatches = true

class CustomMismatchError < Scientist::Experiment::MismatchError
  def to_s
    message = "There was a mismatch! Here's the diff:"
    diffs = result.candidates.map do |candidate|
      Diff.new(result.control, candidate)
    end.join("\n")
    "#{message}\n#{diff}"
  end
end

science "widget-permissions" do |e|
  e.use { Report.find(id) }
  e.try { ReportService.new.fetch(id) }
  e.raise_with CustomMismatchError
end

Scientist::Observation::RESCUES.replace [StandardError]

class MyExperiment
  include Scientist::Experiment
  def raised(operation, error)
    InternalErrorTracker.track "science failure in #{name}: #{operation}", error
  end
end

require "science"
class MyWidget
  include Scientist
  def allow?(user)
    science "widget-permissions" do |e|
      e.use { model.check_user(user).valid? }
      e.try("api") { user.can?(:read, model) }
      e.try("raw-sql") { user.can_sql?(:read, model) }
    end
  end
end

experiment = MyExperiment.new("various-ways") do |e|
  e.try("first-way") { ... }
  e.try("second-way") { ... }
end

science "various-ways", run: "first-way" do |e|
  e.try("frist-way") { ... }
  e.try("secnod-way") { ... }
end

Scientist.run "widget-permissions" do |e|
  e.use { model.check_user(user).valid? }
  e.try { user.can>(:read, model) }
end

```

```
```


