require 'yaml'
require 'digest/md5'


STORE  = 'store/appstore'
IMAGES = 'screenshot_*.{png,jpg,jpeg}'
LIST   = 'screenshots.yml'


def git(*args)
  out = IO.popen(['git', '-C', __dir__, *args], &:read)
  raise "failed to run git #{args.join ' '}" unless $?.success?
  out.strip
end


desc "write #{STORE}/<language>/#{LIST} here, or in root=DIR," +
  ' listing the screenshots here at the URLs of HEAD'
task :yml do
  root  = ENV['root'] ? File.expand_path(ENV['root'], Rake.original_dir) : __dir__
  store = File.join root, STORE
  raise "no #{store}" unless File.directory? store

  raise "changes not committed in #{__dir__}" unless git('status', '--porcelain').empty?
  commit = git 'rev-parse', 'HEAD'
  raise "#{commit} not pushed to origin, or not fetched" if
    git('branch', '-r', '--contains', commit, '--list', 'origin/*').empty?
  repo   = git('remote', 'get-url', 'origin')[%r{github\.com[:/](.+?)(?:\.git)?\z}, 1] ||
    raise('origin not on GitHub')

  shots   = Dir.glob(File.join('*', '*', IMAGES), base: File.join(__dir__, STORE))
    .sort_by {[File.dirname(_1), File.basename(_1)[/\d+/].to_i]}
    .group_by {_1.split('/').first}
  missing = shots.keys.reject {File.directory? File.join(store, _1)}
  raise "no #{missing.join ', '} in #{store}" unless missing.empty?

  shots.each do |lang, paths|
    path = File.join store, lang, LIST
    File.write path, paths.map {|shot|
      {
        'path' => shot.delete_prefix("#{lang}/"),
        'url'  => "https://raw.githubusercontent.com/#{repo}/#{commit}/#{STORE}/#{shot}",
        'md5'  => Digest::MD5.file(File.join __dir__, STORE, shot).hexdigest
      }
    }.to_yaml.delete_prefix("---\n")
    puts "wrote #{path}"
  end

  # the screenshots of the languages gone, not to be uploaded on
  Dir.glob(File.join store, '*', LIST).each do |path|
    next if shots.key? File.basename(File.dirname path)
    File.delete path
    puts "deleted #{path}"
  end
end
