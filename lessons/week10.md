ActiveRecord Query Interface

Highly recommended reading: http://guides.rubyonrails.org/active_record_querying.html

Many navigation and search functionalities in applications can be boiled down to database queries.

Quick re-introduction of how our Post, Tag, and Categorization models are related:
Post attributes: `id`, `title`, `body`, `user_id`, `created_at`, `updated_at`
Tag attributes: `id`, `title`, `created_at`, `updated_at`
Categorization attributes: `id`, `post_id`, `tag_id`

For example, in our Blog app we might want to show
  a) Links to posts by date posted - so clicking 'September 2015' would bring the user to a list of posts made in September 2015
  b) A list of tags that have been applied to more than 5 different posts - and clicking one would take the user to a list of all the posts with that tag

If you've taken a database class, you might be able to imagine how those SQL queries might look. If you don't have prior database experience, it's still simple to see the parameters that we're looking for, in plain terms: 
  1) for a given date range (ex. Sept. 01, 2015 - Sept. 30, 2015), we want all posts that were created on or after the beginning date *and* created before or on the ending date
  2) First, we need to get a list of tags which are associated with more than 5 posts. So we'd look for Tags that are connected to more than 5 Categorization records. Then, we'd need to find Posts based on a tag - so from a tag, get all corresponding Categorizations, then get the Post from each Categorization.

Rails helps a lot when you want to retreive database records based on queries like these. When you define a model that
