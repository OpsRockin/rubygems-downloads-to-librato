require 'gems'
require 'librato/metrics'
require 'logger'

LIBRATO_USER = ENV['LIBRATO_USER']
LIBRATO_APIKEY = ENV['LIBRATO_APIKEY']
TARGET_GEMS = ENV['TARGET_GEMS'].split(",")

Librato::Metrics.authenticate LIBRATO_USER, LIBRATO_APIKEY
@logger = Logger.new($stdout)

task :default do
  queue = Librato::Metrics::Queue.new
  TARGET_GEMS.map do |tg|
    geminfo = Gems.info(tg)
    queue.add "rubygems.downloads.#{tg}".to_sym => geminfo['downloads']
  end
  @logger.info queue.inspect
  queue.submit
end

