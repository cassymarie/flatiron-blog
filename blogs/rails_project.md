[HOME](../README.md)


---

# Rails Project Review; Upgrading from Sinatra to Rails

## A little intro about this project...

For my Sinatra project, I started to develop a Jobsite Project App, something that could be utilized for the small manufacturing company I work for.  The thought behind it was for the Field foreman to be able to keep track of all the different jobs that are being worked on at the current jobsite.  They will be able to enter in weekly time and have the data for reporting purposes.  However, I am still learning - so it is a work in progress. 

*However!* Since I took a strectch in the last project (tons of nested routes and has_many through associations) ~ I decided to continue on with this project for the Rail requirements.  Initially I thought it would have been easy, I mean I already had the setup/view/controllers forms - how hard could it be?

## My Nested Resourcse 

Figuring out my nested routes was definately an adventure! I wanted my routes to make sense for the *singular* and *plural* routing.  It took me a minute playing around with `rails routes` to establish nestings.  My thought was that since all my employees and jobs are connected to a jobsite, to have them all nested within a `jobsite`.

```ruby
  resources :jobsites, only: [:index, :show]
  resources :jobsite, only: [:new, :edit, :create], controller: 'jobsites' do 
    
    resources :jobs, only: [:index, :create], shallow: true
    get 'jobs/by_hours' => 'jobs#by_hours'
    get 'jobs/by_employees' => 'jobs#by_employees'
    get 'jobs/by_areas' => 'jobs#by_areas'

    resources :job, only: [:new, :update, :edit], controller: 'jobs'
    post '/new_job_area' => 'jobs#new_area'
    delete 'job/:id' => 'jobs#remove'

    resources :employees, only: [:index], shallow: true
    get 'employees/by_hours' => 'employees#by_hours'
    get 'employees/by_jobs' => 'employees#by_jobs'

    resources :jobsite_employees, only: [:new, :create, :destroy], as: 'employee', controller: 'jobsite_employees'

    resources :time_entries, only: [:index], shallow: true

    #To be setup later
    #resources :tasks, only: [:new, :create, :edit, :update, :destroy], shallow: true
  end
  ```
I learned the hard way about the difference with `resources` to `resource` and why my routes were not showing up as initially though (i.e. /jobsite/#/job/#). When writing `resource :job`  my server route became `jobsite/#/job.#` but was not routing correctly to my controller.  To be honest, I just changed it back to `resources` and kept going.

## I loved my Helpers! 
  It was an inital pain to instantiate my jobsite_id for some controllers it was `params[:id]` for others it was `params[:jobsite_id]`.  I ended up taking care of this by creating an `ApplicationHelper` method `current_jobsite` because I was sick and tired of writing `@jobsite = Jobsite.find_by(id: params[:id])`, ENOUGH!!!

```Ruby
#controllers/jobsite_controller.rb
include ApplicationHelper
before_action :current_jobsite

#helpers/application_helper.rb
    def current_jobsite
        @jobsite = Jobsite.find_by(id: params[:jobsite_id]) || Jobsite.find_by(id: params[:id])
    end
```

I did utitlize a few other helpers and got lost in creating some crazy view helper that were pretty much one Gigantic string! I created a helper to generate a table for my jobsite jobs.  Since I was going to utilize this table in a few locations, I wanted to take in a few paramaters of what I wanted to show in my table and created the following helper.

```Ruby
def render_jobs_table(customers: true, areas: true)
    return unless @jobsite.jobs.size > 0 
    @jobsite.site_areas.nil? ? areas = false : areas

    render_table = html_escape('')
    row_data = html_escape('')
    tbody_data = html_escape('')

    hkey = ['Job#', 'Job Name']
    hkey << 'Customers' unless !customers
    @jobsite.site_areas.each {|a| hkey << a.code } unless !areas 
    hkey.each { |key|row_data << content_tag(:th, key.to_s) }
    thead = content_tag(:thead, content_tag(:tr, row_data))

    render_table << thead

        @jobsite.jobs.each { | job | row_data = html_escape('')

            row_data << content_tag(:td, job.job_number) 
            row_data << content_tag(:td, job.name)
            row_data << content_tag(:td, job.customer) unless !customers
            @jobsite.site_areas.each {|a| row_data << content_tag(:td, job.areas.include?(a) ? 'X' : '') } unless !areas 

            tbody_data << content_tag(:tr, row_data)
        }

    tbody = content_tag(:tbody, tbody_data)
    render_table <<  tbody
    content_tag(:table, render_table, class: "table")
end
```

This worked really well and I was impressed with myself that I got it to work.  However, I ended up deciding to create a partial that rendered a table that worked for both my Jobs & Employees, and would react based on the collection passed through, as well as the action route that was calling it.  With spending the time on this rails viewhelper, only made me want to jump right into Javascript. (*guess you'll see my progressive magic on the Javascript Interview*)

