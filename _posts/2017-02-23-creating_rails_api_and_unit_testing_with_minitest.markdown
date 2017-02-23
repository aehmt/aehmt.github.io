---
layout: post
title:  "Creating Rails API and unit testing with Minitest"
date:   2017-02-23 17:34:00 +0000
---


The easiest way to create an API in Rails and get it up and running is creating it with scaffold and testing it with Mini Test.

We will use Google-Ads API’s structure to create our API. Google-Ads has Campaings and every campaign has many Adgroups and every Adgroup has many Ads.

First we need to create our application using ```rails new …``` command. We need API-only application and we will  use Postgres as our database type.

```
rails new Ads --api —database=postgresql
```

Once we are done with creating the application. We can scaffold our models and database relations. We will start with Campaigns since it is the topmost parent model in our app.

```
rails g scaffold campaign name status bid:integer advertising_channel_type

```


Now we can go ahead and scaffold our Adgroups.

```
rails g scaffold adgroup name status bid_amount:integer campaign:references

```

And lastly we can scaffold our Ads.

```
rails g scaffold ad name adgroup:references headline_part1 headline_part2 description path1 path2 final_urls

```

The reason why we use ```…:references``` instead of ‘ids’ is that references will add belongs_to relationship to our models. But we still need to create has_many relationship in our relevant models.

At this point next step is to serialize the data. Luckily we don't have to do much Rails created serializers for us only thing we need to do is to include nested associations if we need them.

```
class CampaignSerializer < ActiveModel::Serializer
  attributes :id, :name, :status, :budget, :advertising_channel_type
  has_many :adgroups, include_nested_associations: true
end


class AdgroupSerializer < ActiveModel::Serializer
  attributes :id, :name, :status, :bid_amount
  has_one :campaign
  has_many :ads, include_nested_associations: true
end


class AdSerializer < ActiveModel::Serializer
  attributes :id, :name, :headline_part1, :headline_part2, :description, :path1, :path2, :final_urls
  has_one :adgroup
end
```

Now we can start writing our tests using Minitest. We have some hardcoded data as examples.


**Campaigns Controller Tests**
```
require 'test_helper'

class CampaignsControllerTest < ActionDispatch::IntegrationTest
  Campaign.destroy_all
  campaigns = [
    {
      :name => "Interplanetary Cruise #%d" % (Time.new.to_f * 1000).to_i,
      :status => 'PAUSED',
      :budget => 44,
      :advertising_channel_type => 'SEARCH'
    },
    {
      :name => "Interplanetary Cruise banner #%d" % (Time.new.to_f * 1000).to_i,
      :status => 'PAUSED',
      :budget => 23,
      :advertising_channel_type => 'DISPLAY'
    }
  ]

      # campaigns.each do |x|
      #   post campaigns_url, params: { campaign: x}, as: :json
      # end
 # setup do
  #   campaigns.each do |x|
  #     Campaign.create(x)
  #   end
  # end

  test "should get index" do
    get campaigns_url, as: :json
    assert_response :success
  end

  test "should create campaign" do
    assert_difference 'Campaign.count', 2 do
    # binding.pry
      campaigns.each do |x|
        post campaigns_url, params: { campaign: x}, as: :json
      end
    end

    assert_response 201
  end

  test "should show campaign" do
    Campaign.all.each do |x|
      get campaign_url(x), as: :json
      assert_response :success
    end
  end

end
```


**Adgroups Controller Tests**
```
require 'test_helper'

class AdgroupsControllerTest < ActionDispatch::IntegrationTest
  Campaign.destroy_all
  Adgroup.destroy_all

  campaigns = [
    {
      :name => "Interplanetary Cruise #%d" % (Time.new.to_f * 1000).to_i,
      :status => 'PAUSED',
      :budget => 44,
      :advertising_channel_type => 'SEARCH'
    },
    {
      :name => "Interplanetary Cruise banner #%d" % (Time.new.to_f * 1000).to_i,
      :status => 'PAUSED',
      :budget => 23,
      :advertising_channel_type => 'DISPLAY'
    }
  ]

  campaigns.each do |x|
    Campaign.create(x)
  end

  ad_groups = [
    {
      :name => "Earth to Mars Cruises #%d" % (Time.new.to_f * 1000).to_i,
      :status => 'ENABLED',
      :campaign_id => Campaign.all.last.id,
      :bid_amount => '8'
    },
    {
      :name => 'Earth to Pluto Cruises #%d' % (Time.new.to_f * 1000).to_i,
      :status => 'ENABLED',
      :campaign_id => Campaign.all.last.id,
      :bid_amount => '4'
    }
  ]

  # setup do
  #   @adgroup = adgroups(:one)
  # end

  test "should get index" do
    get adgroups_url, as: :json
    assert_response :success
  end

  test "should create adgroup" do
    assert_difference 'Adgroup.count', 2 do
      ad_groups.each do |x|
        post adgroups_url, params: { adgroup: x}, as: :json
        # binding.pry
      end
    end

    assert_response 201
  end

  test "should show adgroup" do
    Adgroup.all.each do |x|
      get adgroup_url(x), as: :json
      assert_response :success
    end
  end
end
```



**Ads Controller Tests**
```
require 'test_helper'

class AdsControllerTest < ActionDispatch::IntegrationTest
  expanded_text_ad = [
    {
      :xsi_type => 'ExpandedTextAd',
      :ad_group_id => Adgroup.all.last,
      :headline_part1 => 'Cruise to Mars #%d' % (Time.new.to_f * 1000).to_i,
      :headline_part2 => 'Best Space Cruise Line',
      :description => 'Buy your tickets now!',
      :final_urls => ['http://www.example.com/%d' ],
      :path1 => 'all-inclusive',
      :path2 => 'deals'
    },
    {
      :xsi_type => 'ExpandedTextAd',
      :ad_group_id => Adgroup.all.last,
      :headline_part1 => 'Cruise to Mars #%d' % (Time.new.to_f * 1000).to_i,
      :headline_part2 => 'Best in the galaxy',
      :description => 'Buy your tickets now!',
      :final_urls => ['http://www.example.com/%d' ],
      :path1 => 'all-inclusive',
      :path2 => 'deals'
    }
  ]

  # setup do
  #   @ad = ads(:one)
  # end

  test "should get index" do
    get ads_url, as: :json
    assert_response :success
  end

  test "should create ad" do
    assert_difference 'Ad.count', 2 do
      expanded_text_ad.each do |x|
        post ads_url, params: { ad: x}, as: :json
      end
    end

    assert_response 201
  end

  test "should show ad" do
    Ad.all.each do |x|
      get ad_url(x), as: :json
      assert_response :success
    end
  end

end
```

If we run tests with ```rake tests``` all tests should be passing.

```
Run options: --seed 55449

# Running:

.........

Finished in 0.671064s, 13.4115 runs/s, 16.3919 assertions/s.

9 runs, 11 assertions, 0 failures, 0 errors, 0 skips
```

That's all. Rails lets us create an API and test it with some basic tests in a short period of time. 
