### Arttoo
---
.rb
https://github.com/hybridgroup/artoo

.js
https://github.com/medialab/artoo

http://medialab.github.io/artoo/settings/

```ruby
require 'artoo'
connection :arduino, :adapter => :firmata, :port => '/dev/ttyACM0'
device :led, :driver => :led, :pin => 13
device :button, :driver => :button, :pin => 2
work do
  on button, :push => proc {led.toggle}
end

require 'artoo'
connection :ardrone, :adaptor => :ardrone
device :drone, :driver => :ardrone
work do
  drone.start
  drone.take_off
  after(25.seconds) { drone.hober.land }
  after(25.secnods) { drone.stop }
end

require 'artoo/robot'
SPHEROS = ["", "", "", "", ""]
class SpheroRobot < Artoo::Robot
end
robots = []
SPHEROS.each {|p|
  robots << SpheroRobot.new(:connections =>
                              {:sphero =>
                                {:port => p}})
}
SpheroRobot.work!(robots)

require 'artoo'
connection :loop
device :passthru
api :host => '127.0.0.1', :port => '4321'
work do
  puts "Hello from the API running at #{api_host}:#{api_port}"
end

require './test_helper'
require './test_robot'
describe 'sphero' do
  let(:robot){ Artoo::MainRobot.new }
  let(:start) { Time.now }
  before :each do
    Timecop.travel(start)
    robot.work
  end
  after :each do
    Timecop.return
  end
  it 'has work to do every 3 seconds' do
    robot.has_work?(:every, 3.seconds).wont_be_nil
  end
  it 'receives collision event' do
    robot.expects(:contact)
    robot.sphero.publish("collision", "clunk")
    sleep 0.05
  end
  it 'must roll every 3 seconds' do
    Timecop.travel(start + 3.seconds) do
      robot.sphero.expects(:roll)
      sleep 0.05
    end
    Timecop.travel(start + 6.seconds) do
      robot.sphero.expects(:roll)
      sleep 0.05
    end
  end
end

require 'artoo'
connection :sphero, :adaptor => :sphero, :port => '127.0.0.1:4500'
device :sphero, :driver => :sphero
def contact(*args)
  @contacts ||= 0
  @contacts += 1
  puts "Contact #{@contacts}"
end
work do
  on sphero, :collision => :contact
  every(3.seconds) do
    sphero.roll 90, rand(360)
  end
end
```

```
rvm get head && rvm install rbx-2.1.1 --1.9
gem install artoo
gem install artoo-joystick
gem install artoo-ardrone
gem install hybridgroup-serialport
ruby myrobot.rb

artoo
artoo console ./examples/hello.rb
hello
stop
exit
artoo g adaptor awesome_device
```

```sh
npm install
npm test
gulp bookmarklets
npm start
npm run https
```

```js
artoo.settings.debug = true;

artoo.loadSettings({
  debug: false
});

asybnc.series(task);
artoo.deps.aysnc.series(task);

```

```json
{
  "dependencies": [
    {
      "name": "async",
      "url": "//cdnjs.cloudflare.com/ajax/libs/async/0.9.0/async.js",
      "globals": ["async"]
    ]
  }
}
```
