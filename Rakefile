require 'rake/clean'
require 'json'

PREFIX = 'braille-ja-table'
SOURCE_TSV = 'source.tsv' # raw japanese => escaped braille
SOURCES = [SOURCE_TSV, 'Rakefile']

CLOBBER.include %w[escaped raw].product(%w[tsv json]).map{|f,s| "#{PREFIX}-#{f}.#{s}"}

task default: %w[tsv json]
task tsv: %w[raw escaped].map{|suffix| "#{PREFIX}-#{suffix}.tsv"}
task json: %w[raw escaped].map{|suffix| "#{PREFIX}-#{suffix}.json"}

def escape(s) = s.dump[1..-2].downcase
def unescape(s) = %'"#{s}"'.undump
def json(h) = JSON.pretty_generate(h, indent: '  ')

SOURCE_TABLE = File.foreach(SOURCE_TSV).inject({}) do |acc, line|
  *list = line.chomp.split("\t")
  if list.size == 2
    ja, br_escaped = *list
    acc.merge!(ja => br_escaped.scan(/.{6}/).map{|s| unescape(s)}.join)
  end
  acc
end

desc 'create raw TSV'
file "#{PREFIX}-raw.tsv" => SOURCES do |target|
  tsv = SOURCE_TABLE.map {|pair| pair.join("\t")}.join("\n")
  File.write(target.name, tsv << "\n")
end

desc 'create escaped TSV'
file "#{PREFIX}-escaped.tsv" => SOURCES do |target|
  tsv = SOURCE_TABLE.map {|pair| pair.map{|s| escape(s)}.join("\t")}.join("\n")
  File.write(target.name, tsv << "\n")
end

desc 'create raw JSON'
file "#{PREFIX}-raw.json" => SOURCES do |target|
  File.write(target.name, json(SOURCE_TABLE))
end

desc 'create escaped JSON'
file "#{PREFIX}-escaped.json" => SOURCES do |target|
  table = SOURCE_TABLE.inject({}) do |acc, pair|
    e = pair.map{|s| escape(s)}
    acc.merge!([e].to_h)
  end
  File.write(target.name, json(table).gsub('\\' * 2, '\\'))
end
