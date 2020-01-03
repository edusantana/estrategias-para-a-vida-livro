
SOURCE_DIR = 'livro'
MAIN_FILENAME = 'livro.adoc'
WIP_FILENAME = 'wip.adoc'  # work in progress

MAIN_PATH = "#{SOURCE_DIR}/#{MAIN_FILENAME}"
WIP_PATH = "#{SOURCE_DIR}/#{WIP_FILENAME}"

# Desativa WIP se passar WIP=0
USE_WIP = ENV['WIP'] != '0'

fonte = USE_WIP ? WIP_PATH : MAIN_PATH

RELEASE_DIR = 'release'
directory RELEASE_DIR

desc 'Gerando em livro html'
task 'html' => [RELEASE_DIR] do

  sh "asciidoctor #{fonte} -D #{RELEASE_DIR} -o livro.html"

end

desc 'Gerando em livro epub'
task 'epub' => [RELEASE_DIR] do
end

desc 'Gerando em livro pdf'
task 'pdf' => [RELEASE_DIR] do
  sh "asciidoctor-pdf #{fonte} -D #{RELEASE_DIR} -o livro.pdf"
end

desc 'Geração de PDF através do docbook'
task :docbook do
  Dir.chdir SOURCE_DIR do
    sh "asciidoctor wip.adoc -b docbook5 -o livro.docbook5.xml"
    sh "dblatex -p ../config/dblatex/asciidoc-dblatex.xsl -s ../config/dblatex/asciidoc-dblatex.sty livro.docbook5.xml"
    sh "xsltproc --xinclude http://bbcarchdev.github.io/docbook-html5/docbook-html5.xsl livro.docbook5.xml > livro.docbook5.html"
    mv "livro.docbook5.xml", "../#{RELEASE_DIR}"
    mv "livro.docbook5.pdf", "../#{RELEASE_DIR}"
  # -D #{RELEASE_DIR}
    # -T computacao
    #sh "dblatex -P latex.babel.language=brazilian -P preface.tocdepth=1 -P doc.collab.show=0 livro.docbook5.xml"

  end
end

namespace 'docker' do

  #desc 'Constroi imagem docker'
  task 'build' do
  end
end

namespace "wip" do

  desc "Create new wip file from book source"
  task "new" do
    cp MAIN_PATH, WIP_PATH
  end

  desc "Replaces #{MAIN_PATH} with #{WIP_PATH}"
  task :invert do
    cp WIP_PATH, MAIN_PATH
  end

end

file WIP_PATH => [MAIN_PATH] do
  Rake::Task["wip:new"].invoke
end

desc "Print workflow"
task :work do
  # Para atualizar use: http://bloodgate.com/perl/graph/manual/overview.html
  sh "graph-easy livro/imagens/workflow.txt"
end

namespace :i do
  desc "install fop docbook-xsl-doc-pdf"
  task :deps do
    puts "https://help.ubuntu.com/community/DocBook"
    sh "fop docbook-xsl-doc-pdf"
  end

  desc "install dblatex"
  task :dblatex do
    sh "sudo apt install dblatex texlive-lang-portuguese"
  end

  task :docs do
    sh "sudo apt install docbook-defguide"
  end

  task :epub3 do
    puts "https://asciidoctor.org/docs/asciidoctor-epub3/"
    sh "sudo apt-get install libxslt-dev libxml2-dev"
    sh "NOKOGIRI_USE_SYSTEM_LIBRARIES=1 gem install asciidoctor-epub3 --pre
"
  end
end

task "help:docbook" do
  sh "xdg-open /usr/share/doc/docbook-defguide/html/docbook.html"
  sh "xdg-open https://help.ubuntu.com/community/DocBook"
end
