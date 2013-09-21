require 'rake/clean'
require 'json'

PREFIX = 'braille-ja-table'
SOURCE = ["#{PREFIX}-source.tsv", # raw japanese => escaped braille
          'Rakefile']

CLOBBER.include %w[escaped raw].product(%w[tsv json]).map{|f,s| "#{PREFIX}-#{f}.#{s}"}

task default: %w[tsv json]
task tsv: %w[raw escaped].map{|suffix| "#{PREFIX}-#{suffix}.tsv"}
task json: %w[raw escaped].map{|suffix| "#{PREFIX}-#{suffix}.json"}

def escape(s)
  s.unpack('U*').map{|n| '\u' + n.to_s(16)}.join
end

def unescape(s)
  [s[2..-1].to_i(16)].pack('U*')
end

def json(h)
  JSON.pretty_generate(h, indent: '  ')
end

SOURCE_TABLE = IO.foreach(SOURCE.first).inject({}) do |acc, line|
  *list = line.chomp.split
  if list.size == 2
    ja, br_escaped = *list
    acc.merge!(ja => br_escaped.scan(/.{6}/).map{|s| unescape(s)}.join)
  else
    acc
  end
end

desc 'create raw tsv'
file "#{PREFIX}-raw.tsv" => SOURCE do |target|
  tsv = SOURCE_TABLE.map {|pair| pair.join("\t")}.join("\n")
  IO.write(target.name, tsv << "\n")
end

desc 'create escaped tsv'
file "#{PREFIX}-escaped.tsv" => SOURCE do |target|
  tsv = SOURCE_TABLE.map {|pair| pair.map{|s| escape(s)}.join("\t")}.join("\n")
  IO.write(target.name, tsv << "\n")
end

desc 'create raw json'
file "#{PREFIX}-raw.json" => SOURCE do |target|
  IO.write(target.name, json(SOURCE_TABLE))
end

desc 'create escaped json'
file "#{PREFIX}-escaped.json" => SOURCE do |target|
  table = SOURCE_TABLE.inject({}) do |acc, pair|
    e = pair.map{|s| escape(s)}
    acc.merge!(e[0] => e[1])
  end
  IO.write(target.name, json(table).gsub('\\' * 2, '\\'))
end
