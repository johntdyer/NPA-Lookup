#!/usr/bin/env ruby -WKU
require 'rubygems'
require "net/http"
require "uri"
require 'json/pure'
require 'terminal-table/import' #http://vision-media.ca/resources/ruby/ruby-terminal-table-gem

SPACES = "18"

def colorize(text, color_code,how_many_spaces)
  "\e[#{color_code}m#{text}\e[#{how_many_spaces}C\033[0m\033[0m"
end

def red(text,how_many_spaces); colorize(text, 31,how_many_spaces); end
def green(text,how_many_spaces); colorize(text, 32,how_many_spaces); end
def yellow(text,how_many_spaces); colorize(text, 33,how_many_spaces); end
def blue(text,how_many_spaces); colorize(text, 34,how_many_spaces); end
def cyan(text,how_many_spaces); colorize(text, 36,how_many_spaces); end
def white(test,how_many_spaces);colorize(test,37,how_many_spaces);end

def parse_args(i)
  i=i.to_s
  if i.length==3
    return i
  elsif i.length==6
    i=i.dup
    i.insert(3, '/')
  elsif i.length==10
    return i
  else
    puts "Error {NumberError}"
    exit!
  end
end

ARGV.each do|a|
    uri = URI.parse("http://digits.cloudvox.com/#{parse_args(a)}.json")
    response = Net::HTTP.get_response(uri)
  begin
    jsonObject = JSON.parse(response.body)
  rescue JSON::ParserError => msg
    puts "Failed: #{msg.message} | #{a}.json"
    exit!
  end

    data = jsonObject['allocation']
    
    t = table ['Data', 'Value']
    t << [red("NPA","18"),yellow("#{data['npa']}",SPACES)]

    if data['nxx']!=nil; t << [red("Nxx","18"),yellow("#{data['nxx']}",SPACES)];end

    if data['ratecenter_formatted'] !=nil&&data['ratecenter_formatted']!=""; t << [red("City","18"),yellow("#{data['ratecenter_formatted']}",SPACES)]; end

    t << [red("State","18"),yellow("#{data['region']}",SPACES)]
    t << [red("Status","18"),yellow("#{data['status'].capitalize}",SPACES)]

    if data['carrier_name'] !=nil; t << [red("Carrier","18"),yellow("#{data['carrier_name']}",SPACES)]; end

    puts t 
    puts "\n\n"
end
