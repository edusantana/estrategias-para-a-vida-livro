
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
  sh "asciidoctor #{fonte} -b docbook5 -D #{RELEASE_DIR} -o livro.docbook5.xml"
  Dir.chdir RELEASE_DIR do
    # -T computacao
    #sh "dblatex -P latex.babel.language=brazilian -P preface.tocdepth=1 -P doc.collab.show=0 livro.docbook5.xml"

    sh "dblatex -p ../config/dblatex/asciidoc-dblatex.xsl -s ../config/dblatex/asciidoc-dblatex.sty livro.docbook5.xml"
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

  file WIP_PATH => [MAIN_PATH] do
    Rake::Task["wip:new"].invoke
  end


end

desc "Print workflow"
task :work do
  # Para atualizar use: http://bloodgate.com/perl/graph/manual/overview.html
  sh "graph-easy livro/images/workflow.txt"
end
