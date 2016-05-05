require 'apple_retail_store_part_availability'
require 'mail'
require 'json'

Mail.defaults do
  delivery_method :smtp, {
    address: 'smtp.sendgrid.net',
    port: 587,
    domain: 'heroku.com',
    user_name: ENV['SENDGRID_USERNAME'],
    password: ENV['SENDGRID_PASSWORD'],
    authentication: :plain,
    enable_starttls_auto: true
  }
end


desc "Check if iPhone SE is available in Lincoln Park"
task :check do
  service = AppleRetailStorePartAvailability::Service.fetch(part_id: 'MLMD2LL/A', store_id: 'R284')
  available = service.is_available?


  if available
    response = service.response['body']['stores'].find { |s| s['storeNumber'] == 'R284' }['partsAvailability']['MLMD2LL/A']

    Mail.deliver do
      to ENV['RECEIVER_EMAIL']
      from 'noreply@example.com'
      subject '[ALERT]: There is an iPhone SE in Lincoln Park!'
      body JSON.pretty_generate(response)
    end
  end
end
