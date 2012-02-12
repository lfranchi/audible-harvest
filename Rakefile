require 'active_record'
require 'uri'

require 'server/lib/treasure'

namespace :server do

  desc "Migrate the database through scripts in server/migrate. Target specific version with VERSION=x"
  task :migrate => :environment do
    ActiveRecord::Migrator.migrate('server/migrations', ENV["VERSION"] ? ENV["VERSION"].to_i : nil )
  end

  desc "Deletes all TEST origin treasure from the db."
  task :clean => :environment do
    Treasure.where( :origin => 'TEST' ).destroy_all
  end

  task :environment do
    db = URI.parse(ENV['DATABASE_URL'] || 'postgres://localhost/mydb')

    ActiveRecord::Base.establish_connection(
      :adapter  => db.scheme == 'postgres' ? 'postgresql' : db.scheme,
      :host     => db.host,
      :username => db.user,
      :password => db.password,
      :database => db.path[1..-1],
      :encoding => 'utf8'
    )
  end

end
